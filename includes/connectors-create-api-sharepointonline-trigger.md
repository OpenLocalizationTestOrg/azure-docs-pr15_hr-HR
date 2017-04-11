U ovom primjeru I vidjet ćete kako pomoću **Sustava SharePoint Online – prilikom stvaranja nove stavke** okidač pokrenuti tijek rada logike aplikacije prilikom stvaranja nove stavke na popis sustava SharePoint Online.

>[AZURE.NOTE]Od vas će se zatražiti da biste se prijavili na račun sustava SharePoint, ako već niste stvorili *vezu* na SharePoint Online.  

1. U okvir za pretraživanje unesite *sustava sharepoint* u dizajneru logike aplikacije, a zatim odaberite **SharePoint Online – prilikom stvaranja nove stavke** okidača.  
![SharePoint online okidača slike](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- Prikazat će se kontrolu **prilikom stvaranja nove stavke** .  
![SharePoint online okidača slika 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Odaberite **URL web-mjesta**. Ovo je sustava SharePoint online web-mjesta koje želite nadzirati za nove stavke da biste pokrenuli tijek rada.  
![Slika sustava SharePoint online okidača 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Odaberite **naziv popisa**. Ovo je popis na web-mjestu sustava SharePoint Online želite nadzirati za nove stavke koji će pokrenuti tijek rada.  
![SharePoint online okidača slika 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Sada aplikacijom logike je konfiguriran pomoću okidača koji će se početi Pokreni drugi okidača i akcija tijeka rada. To se odvija prilikom svakog stvaranja nove stavke na popis sustava SharePoint Online koji ste odabrali.  