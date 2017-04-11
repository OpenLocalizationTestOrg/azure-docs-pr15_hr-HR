Da biste stvorili na VNet u modelu implementaciju upravljanja resursima pomoću portala za Azure, slijedite korake u nastavku. U snimke zaslona prikazuje se kao primjere. Provjerite vrijednosti zamijeniti vlastitim. Dodatne informacije o radu s virtualne mreže potražite u članku [Pregled virtualne mreže](../articles/virtual-network/virtual-networks-overview.md).

1. U pregledniku idite na [portal za Azure](http://portal.azure.com) i, ako je potrebno, prijavite se pomoću računa za Azure.

2. Kliknite **Novo**. U polje za **pretraživanje na tržištu** upišite "Virtualne mreže". Pronađite **Virtualne mreže** na popisu vraćeni i kliknite da biste otvorili plohu **Virtualne mreže** .

    ![Pronađite virtualne mreže resurse plohu] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Pronađite virtualne mreže resurse plohu")

3. Pri dnu plohu virtualne mreže, s popisa **Odaberite model implementacije** odaberite **Resursima**pa zatim kliknite **Stvori**.


    ![Odaberite upravitelj resursa] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Odaberite upravitelj resursa")

4. Postavke VNet konfigurirati na plohu **Stvaranje virtualne mreže** . Kad ispunite polja crveni uskličnik će postati zelenu kvačicu kada su valjani znakovi unijete u polje.

    ![Provjera valjanosti polja] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Provjera valjanosti polja")

5. **Stvaranje virtualne mreže** plohu izgleda slično sljedećem primjeru. Možda postoje vrijednosti koje su automatski ispunjava. Ako je tako, zamijenite vrijednosti vlastitu.

    ![Stvaranje virtualne mreže plohu] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Stvaranje virtualne mreže plohu")

6. **Naziv**: upišite naziv za virtualne mreže.

7. **Prostor adrese**: Unesite adresu razmak. Ako imate više razmaka adresu da biste dodali, dodajte prvi adresnog prostora. Kasnije možete dodati dodatne razmake, nakon stvaranja na VNet.
 
8. **Naziv podmreže**: dodajte naziv podmreže i podmreže adresni raspon. Dodatni podmreže možete dodati kasnije, nakon stvaranja na VNet.

10. **Pretplate**: Provjerite je li pretplata navedena ispravnu. Pretplate možete promijeniti pomoću padajući popis.

11. **Grupa resursa**: Odaberite postojeću grupu resursa ili stvorite novi tako da upišete naziv za novu grupu resursa. Ako stvarate novu grupu, naziv grupe resursa prema vrijednosti planiranog konfiguracije. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](resource-group-overview.md#resource-groups).

12. **Lokacija**: Odaberite mjesto za vaše VNet. Mjesto određuje koje će se nalaziti resursa koji implementirati ovaj VNet.

13. Ako želite da biste mogli jednostavno pronaći vaše VNet na nadzornu ploču, a zatim kliknite **Stvori**, odaberite **Prikvači na nadzornoj ploči** .
    
    ![Prikvači na nadzornoj ploči] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "Prikvači na nadzornoj ploči")

14. Nakon što kliknete **Stvori**, vidjet ćete pločicu na nadzornu odražavaju tijeku vaše VNet. Na pločici mijenja kao što je stvoren je u VNet.

    ![Stvaranje virtualne mreže pločica] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Stvaranje virtualne mreže pločica")