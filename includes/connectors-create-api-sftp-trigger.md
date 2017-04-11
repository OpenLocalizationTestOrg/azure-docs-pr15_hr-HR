Dodat ćemo okidač.

1. U okvir za pretraživanje unesite *sftp* u dizajneru logike aplikacije, a zatim odaberite okidač **SFTP - prilikom dodavanja ili promjene datoteke**   
![SFTP okidač slika 1](./media/connectors-create-api-sftp/trigger-1.png)  
- **Prilikom dodavanja ili promjene datoteke** otvara prema gore  
![Slika okidača SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Odaberite **...** koja se nalazi na desnoj strani kontrole. Otvorit će se kontrola alata za odabir mape  
![Slika okidač SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
- Odaberite **SFTP** da biste odabrali korijenske mape kao što je mapa za datoteke koje su nove ili izmijenjene praćenje. Obratite pozornost na korijenskoj mapi sada se prikazuje u kontroli **mape** .  
![Slika okidač SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

Sada logike aplikacije je konfiguriran pomoću okidača koji će početi Pokreni drugi okidača i akcija tijeka rada kada se datoteke izmjene ili stvorene u određenu mapu SFTP. 

>[AZURE.NOTE]Logika App biti funkcionalne, mora sadržavati najmanje jedan okidača i jednu akciju. Slijedite korake iz sljedećeg odjeljka da biste dodali akciju.  