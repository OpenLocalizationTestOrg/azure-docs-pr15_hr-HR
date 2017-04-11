1. Na portalu iz **svih resursa**, kliknite **+ Dodaj**. U **sve** plohu okvir za pretraživanje upišite **pristupnika lokalne mreže**, a zatim kliknite pretraživanje. To će vratiti popis. Kliknite **pristupnik za lokalnu mrežu** da biste otvorili na plohu, a zatim kliknite **Stvori** da biste otvorili plohu **Stvaranje lokalne mreže pristupnika** .

    ![Stvaranje pristupnika za lokalnu mrežu](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Na **Stvaranje lokalne mreže pristupnika plohu**navedite **naziv** objekta pristupnika lokalnoj mreži.
 
3. Navedite valjanu javnu **IP adresa** za VPN uređaj ili virtualne mreže pristupnika na koju se želite povezati.<br>Ako lokalne mreže predstavlja lokalne lokacije, to je javnu IP adresu VPN uređaja na kojem želite povezati. Nije moguće iza NAT i mora biti dostupna tako da Azure.<br>Ako lokalne mreže predstavljaju drugi VNet, bit će naveden javnu IP adresu koja je dodijeljena pristupnika virtualne mreže za tu VNet.<br>

4. **Prostor adrese** se odnosi na rasponi adresa za mrežu koja predstavlja lokalne mreže. Možete dodati više raspona prostora na adresu. Provjerite da raspona koji navedete preklapa s raspona drugim mrežama koje želite povezati.
 
5. Za **pretplatu**, provjerite je li se prikazuje ispravni pretplate.

6. **Grupa resursa**, odaberite grupu resursa koju želite koristiti. Možete stvoriti novu grupu resursa ili odaberite onaj koji ste već stvorili.

7. **Mjesto**odaberite mjesto na koje će se stvoriti taj objekt. Možda ćete morati odabrati isto mjesto koje se nalazi vaš VNet, ali ne zahtijeva da biste to učinili.

8. Kliknite **Stvori** da biste stvorili pristupnik za lokalnu mrežu.
