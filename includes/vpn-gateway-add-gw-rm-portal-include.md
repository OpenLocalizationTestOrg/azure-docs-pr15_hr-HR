1. Na portalu, idite na **Novo**. Upišite "Virtualne mrežni pristupnik" u pretraživanju. Pronađite **virtualne mrežni pristupnik** u pretraživanju povrata i kliknite stavku. Otvorit će se plohu **Stvaranje virtualne mreže pristupnika** .
2. Kliknite **Stvori** pri dnu plohu **virtualne mrežni pristupnik** . Otvorit će se plohu **Stvaranje virtualne mreže pristupnika** . Unesite vrijednosti za pristupnik za virtualne mreže.

    ![Stvaranje virtualne mreže pristupnika plohu polja] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Stvaranje virtualne mreže pristupnika plohu polja")

3. **Naziv**: naziv vaše pristupnika. Ovo nije isti kao imenovanja podmreži pristupnika. To je naziv objekta pristupnika koje stvarate.

4. **Pristupnik vrsta**: odaberite **VPN-a**. VPN pristupnik koriste pristupnika vrstu virtualne mreže **VPN-a**. 

5. **VPN vrsta**: Odaberite vrstu VPN-a koji je naveden za konfiguraciju. Većina konfiguracije zahtijevaju vrstu utemeljen na usmjeravanje VPN-a.

6. **SKU**: na padajućem izborniku odaberite pristupnik SKU-om. SKU-ove naveden na padajućem popisu ovise o vrsti VPN-a odaberete.

7. **Lokacija**: Prilagodba polje **mjesto** tako da pokazuje na mjesto na kojem se nalazi virtualne mreže.
 
8. Odaberite virtualne mreže na koji želite dodati ovaj pristupnika. Kliknite **virtualne mreže** da biste otvorili plohu **Odabir virtualne mreže** . Odaberite na VNet. Ako ne vidite VNet, provjerite je li polje **mjesto** pokazuje na područje u kojem se nalazi virtualne mreže.

9. Odaberite javna IP adresa. Kliknite **javnu IP adresa** da biste otvorili plohu **Odaberite javna IP adresa** . Kliknite **+ Stvori novo** da biste otvorili **Stvaranje javne plohu IP adresa**. Unesite naziv javnu IP adresa. Ovaj plohu stvara javno objekt IP adresu kojoj javnu IP adresu dinamički dodjeljuju.<br>Kliknite **u redu** da biste spremili promjene na ovom plohu.

10. **Pretplate**: Provjerite je li odabran ispravan pretplate.

11. **Grupa resursa**: ta postavka ovisi o virtualne mreže koju ste odabrali. 

12. Ne prilagoditi **mjesto** nakon što ste naveli prethodne postavke.

13. Provjerite postavke. **Prikvači na nadzornu ploču** pri dnu zaslona u plohu možete odabrati ako želite da se pristupnikom da se pojavi na nadzornoj ploči.

14. Kliknite **Stvori** da biste započeli stvaranje pristupnika. Postavke će se provjeriti i vidjet ćete "Deploying virtualne mreže pristupnika" pločica na nadzornoj ploči. Stvaranje pristupnika, može proći do 45 minuta. Možda ćete morati osvježiti stranici portala da biste vidjeli dovršene status.

    ![Implementacija virtualne mrežni pristupnik] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Implementacija virtualne mrežni pristupnik")

11. Nakon stvaranja pristupnika možete pogledati IP adresu koja vam je dodijeljen ga tako da pogledate virtualne mreže na portalu. Pristupnika prikazat će se kao povezani uređaj. Možete kliknuti povezani uređaj (pristupnikom virtualne mreže) da biste pogledali dodatne informacije.



