## <a name="create-an-event-hub"></a>Stvaranje koncentratora za događaj

1. Prijavite se na [portal za Azure][]i kliknite **Novo** u gornjem lijevom kutu zaslona.

2. Kliknite **Podaci + analize**, a zatim kliknite **Događaj koncentratora**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. U plohu **prostor naziva stvaranje** unesite polja naziva. U sustavu odmah provjerava je li dostupna naziv.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Nakon što provjerite polja naziva nije dostupan, odaberite cijene sloju (Basic ili standardna). Osim toga, odaberite je Azure pretplatu, grupa resursa i mjesto na kojem želite stvoriti resurs. 

2. Kliknite **Stvori** da biste stvorili naziva.

6. Na popisu polja naziva događaja koncentratora kliknite novostvorenu naziva.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. U plohu prostor naziva kliknite **Događaj koncentratora**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Pri vrhu na plohu, kliknite **Dodavanje koncentratora za događaj**.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Upišite naziv koncentratora za događaj, a zatim kliknite **Stvori**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Na popisu koncentratora događaja kliknite naziv novostvorenu koncentratora za događaj. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Vratite se u plohu prostor za naziv (ne na određeni događaj koncentrator plohu), kliknite **zajednički se koristi access pravila**, a zatim **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Kliknite gumb Kopiraj da biste kopirali **RootManageSharedAccessKey** niz za povezivanje u međuspremnik. Spremite ovaj niz za povezivanje da biste koristili kasnije u ovom praktičnom vodiču.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Napravili koncentratora za događaj, a imate potrebne za slanje i primanje događaje nizu za povezivanje.

[Portal za Azure]: https://portal.azure.com/