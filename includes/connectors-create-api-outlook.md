## <a name="connect-to-outlookcom"></a>Povezali sa servisom Outlook.com

### <a name="prerequisites"></a>Preduvjeti
- Račun za Outlook.com

Da biste koristili račun za Outlook.com u aplikaciji logike, morate Autorizirajte logike aplikacije da biste se povezali s računom za Outlook.com. Srećom, to možete jednostavno iz unutar aplikacije logike portala za Azure. 

Evo nekoliko koraka da biste autorizirali logike aplikacije da biste se povezali s računom za Outlook.com:

1. Sve aplikacije logike potrebne su vam da se pokrene uslijed okidač pa nakon stvaranja logike aplikacije, otvorit će se dizajner i prikazuje popis pokreće koji možete koristiti da biste pokrenuli aplikaciju logika:

  ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. U okvir za pretraživanje unesite "outlook". Obratite pozornost na popisu filtriran na popisu okidača "Outlook" naziv:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Odaberite **Outlook sustava Office 365 – na nove e-pošte**.   
  Ako niste stvorili sve veze s programom Outlook prije, ćete dobiti upit možete unijeti vjerodajnice za Outlook.com. Te vjerodajnice koristit će se da biste autorizirali logike aplikacije za povezivanje s i pristup podacima na računu servisa Outlook.com:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Navedite vjerodajnice za Outlook i prijavite se u:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
  To je to. Sada ste stvorili veze s programom Outlook. Povezivanje bit će dostupni za upotrebu u druge aplikacije logike koje ste stvorili.


