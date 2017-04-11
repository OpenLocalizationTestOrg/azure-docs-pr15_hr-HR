### <a name="prerequisites"></a>Preduvjeti
- Twilio računa
- Potvrđeni telefonskog broja Twilio koji možete primati SMS PORUKE
- Potvrđeni telefonskog broja Twilio koje možete poslati SMS PORUKE

>[AZURE.NOTE] Ako koristite račun za probno razdoblje Twilio, možete poslati samo SMS da biste **provjerili** telefonskih brojeva.  

Vaš račun Twilio mogli koristiti u aplikaciji logike, morate Autorizirajte aplikaciju logike da biste se povezali s računom Twilio. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure. 

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom Twilio:

1. Da biste stvorili vezu s Twilio, u dizajneru logike aplikacije, na padajućem popisu odaberite **Prikaz Microsoft upravljani API-ji** , zatim u okvir za pretraživanje unesite *Twilio* . Odaberite pokretanje ili akcije koje želite koristiti:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Ako niste stvorili veze Twilio prije, ćete dobiti upit možete unijeti vjerodajnice Twilio. Te vjerodajnice koristit će se da biste autorizirali logike aplikacije možete povezati i pristupiti podacima računa Twilio:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Morat ćete **Twilio id računa** i **token za pristup Twilio** na nadzornoj ploči u Twilio, pa se prijavite Twilio računu sada da biste privukli ove dvije dijelovi informacija:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Aplikacije Twilio i logike koristiti različite nazive za identifikaciju ove dvije dijelovi informacija. Evo kako ih morate mapirati u dijaloškom okviru aplikacije logika:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Odaberite gumb **Stvori vezu** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Obratite pozornost na to stvoreno je veza i sada ste besplatne nastavite s koracima u svojoj aplikaciji logika:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)