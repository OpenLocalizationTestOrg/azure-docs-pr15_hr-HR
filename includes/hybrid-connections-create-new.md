
1. Na računalu na lokalni prijavite se na [Portal za upravljanje Azure](http://manager.windowsazure.com) (to je stari portal).

2. Pri dnu navigacijskog okna, odaberite **+ NOVO** > **Aplikacije servisa** > **BizTalk servis** > **Stvoriti prilagođene**.

3. Navedite **Naziv BizTalk servis** , a zatim odaberite **Edition**. 

    Pomoću ovog praktičnog vodiča koristi **mobile1**. Morat ćete navesti jedinstveni naziv novog BizTalk servisa.

4. Nakon što BizTalk servisa, odaberite karticu **Hibridnog veze** , a zatim kliknite **Dodaj**.

    ![Dodavanje veze za hibridno](./media/hybrid-connections-create-new/3.png)

    Time se stvara novu vezu hibridnog.

5. Unesite **naziv** i **Glavno računalo** za hibridno vezu i postavite **priključak** `1433`. 
  
    ![Konfiguriranje Hibridne veze](./media/hybrid-connections-create-new/4.png)

    Naziv glavnog računala je naziv lokalnog poslužitelja. To konfigurira veze hibridnog pristupa sustava SQL Server na priključak 1433. Ako koristite imenovani instancu sustava SQL Server, umjesto toga koristite statične priključak prethodno definirane.

6. Nakon novu vezu stvaranja status na nove veze prikazuje **Lokalni postavljanje nepotpuna**.

7. Vratite se na servis za mobilne uređaje, kliknite **Konfiguriraj**, pomaknite se do odjeljka **hibridnog veze** i kliknite **vezu Dodaj hibridnog**, zatim odaberite hibridnog vezu koju ste upravo stvorili i kliknite **u redu**.

    Time se omogućuje na servis za mobilne uređaje da biste koristili novi hibridnog vezu.

Nakon toga, morat ćete instalirati upravitelja veze hibridnog na računalu na lokalni.