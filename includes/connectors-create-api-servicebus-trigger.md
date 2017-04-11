Evo kako pomoću **Servisa Bus – kada je poruka primljena u redu čekanja** okidača pokrenuti tijek rada logike aplikacije pri slanju novu stavku servisa Bus red.  

>[AZURE.NOTE]Zatražit će se da se prijavi pomoću niza za povezivanje servisa Bus ako već niste stvorili vezu s Bus servisa.  

1. U okvir za pretraživanje u dizajneru logike aplikacije unesite **bus servisa**. Zatim odaberite pokretanje **Servisa Bus – kada je poruka primljena u redu** .  
![Servis Bus okidača slika 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Prikazat će se dijaloški okvir **se po primitku poruke u redu** .  
![Servis Bus okidač slika 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Unesite naziv reda čekanja Bus servisa koji biste željeli okidač praćenje.   
![Servis Bus okidač slika 3](./media/connectors-create-api-servicebus/trigger-3.png)   

U ovom trenutku logike aplikacije je konfiguriran pomoću okidača. Nova stavka primitku u redu čekanja koji ste odabrali okidač će početi Pokreni drugi okidača i akcija tijeka rada.    
