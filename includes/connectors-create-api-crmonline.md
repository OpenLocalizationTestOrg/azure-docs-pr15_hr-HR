#### <a name="prerequisites"></a>Preduvjeti
- Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)
- Račun sustava [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) 

Prije korištenja računa Dynamics u aplikaciji logike, Autorizirajte logike aplikacije da biste se povezali s računom sustava CRM Online. To možete jednostavno unutar aplikacije logike portala za Azure. 

Autorizirajte logike aplikacije da biste se povezali s računom sustava CRM Online na sljedeći način:

1. Stvaranje logike aplikacije. U dizajneru logike aplikacije na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , a zatim u okvir za pretraživanje unesite "dynamics". Odaberite jednu od okidača ili akcija:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Ako već niste stvorili sve veze sa sustavom Dynamics, morat ćete se prijaviti pomoću vjerodajnica za Dynamics:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Odaberite **Prijava**, a zatim unesite korisničko ime i lozinku. Odaberite **Prijava**. 

    Da biste autorizirali logike aplikacije za povezivanje i pristup podacima na vašem računu Dynamics se koristi te vjerodajnice. 
4. Obratite pozornost na to je stvorena veza. Sada, nastavite s koracima u svojoj aplikaciji logika:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
