### <a name="prerequisites"></a>Preduvjeti

Morate imati račun [Servisa Bus](https://azure.microsoft.com/services/service-bus/) .  

Vaš račun Bus servisa Azure mogli koristiti u aplikaciji logike, morate Autorizirajte logike aplikacije da biste se povezali s računom servisa bus. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure.  

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom servisa Bus:  

1. Da biste stvorili vezu s Bus servisa u dizajneru logike aplikacije, na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** . U okvir za pretraživanje pa unesite **bus servisa** . Odaberite pokretanje ili akciju koju želite koristiti.  
    ![Slika veze servisa Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Ako niste stvorili veze servisa Bus prije, morat ćete unijeti vjerodajnice Bus servisa. Da biste autorizirali logike aplikacije za povezivanje i pristupiti podacima računa servisa Bus se koristi te vjerodajnice. Poveznik za servis Bus treba niz za povezivanje za servis Bus naziva. **Upravljanje** dozvolama i zahtijeva. Dobar način znati ako je niz za povezivanje za prostor za naziv ili određeni entitet sadrži li na `EntityPath` parametar. Ako je tako, nije niz desnom veze za aplikaciju logike.  
    ![Niz za povezivanje servisa Bus](./media/connectors-create-api-servicebus/connectionstring.png)

1. Nakon što ste primili niz za povezivanje za prostor za naziv, možete je koristiti API veze u aplikacijama logike.  
    ![Slika veze servisa Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Obratite pozornost na to stvoreno je veza i sada ste besplatno da biste nastavili s drugim koracima logike aplikacije.  
    ![Slika veze servisa Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
