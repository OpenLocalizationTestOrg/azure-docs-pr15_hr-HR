### <a name="prerequisites"></a>Preduvjeti

- [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) računa  


Vaš račun SMTP mogli koristiti u aplikaciji logike, morate Autorizirajte logike aplikacije da biste se povezali s računom SMTP. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure.  

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom SMTP:  
1. Da biste stvorili vezu s SMTP, u dizajneru logike aplikacije, na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , zatim u okvir za pretraživanje unesite *SMTP* . Odaberite pokretanje ili akcije koje želite koristiti:  
![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Ako niste stvorili veze SMTP prije, ćete dobiti upit možete unijeti vjerodajnice SMTP. Te vjerodajnice koristit će se da biste autorizirali logike aplikacije možete povezati i pristupiti podacima računa SMTP:  
![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Obratite pozornost na to stvoreno je veza i sada ste besplatne nastavite s koracima u svojoj aplikaciji logika:  
 ![](./media/connectors-create-api-smtp/smtp-3.png)  

