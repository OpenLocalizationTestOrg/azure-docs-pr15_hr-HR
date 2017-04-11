U ovom walk-through će Saznajte kako koristiti okidač **Salesforce – stvaranja objekta** za pokretanje tijeka rada logike aplikacije prilikom stvaranja novog potencijalnog klijenta u vašem Salesforce.

>[AZURE.NOTE]Zatražit će se da biste se prijavili na račun servisa Salesforce, ako već niste stvorili *vezu* s Salesforce.  

1. U okvir za pretraživanje unesite *salesforce* u dizajneru logike aplikacije, a zatim odaberite okidač **Salesforce - stvaranja objekta** .  
![Salesforce okidač slika 1](./media/connectors-create-api-salesforce/trigger-1.png)   
- Prikazat će se kontrola **stvaranja objekta** .  
![Salesforce okidač slika 2](./media/connectors-create-api-salesforce/trigger-2.png)   
- Odaberite **Vrstu objekta** , a zatim odaberite *potencijalnog klijenta* s popisa objekata. U ovom ćete koraku koji su koja označava da stvarate okidač će obavijestiti aplikacijom logike prilikom svakog stvaranja novog potencijalnog klijenta u Salesforce.   
![Slika Salesforce okidač 3](./media/connectors-create-api-salesforce/trigger-3.png)   
- To je to. Okidač ste stvorili. Međutim, morati stvoriti barem jedan akcije da biste omogućili valjani logike aplikacije.    
![Salesforce okidač slika 4](./media/connectors-create-api-salesforce/trigger-4.png)   

Sada logike aplikacije je konfiguriran pomoću okidača koji će početi Pokreni drugi okidača i akcija tijeka rada prilikom stvaranja nove stavke u vašem Salesforce.  