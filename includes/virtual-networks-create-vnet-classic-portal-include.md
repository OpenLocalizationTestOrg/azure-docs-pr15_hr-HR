## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Kako stvoriti na VNet na portalu za Azure

Da biste stvorili VNet ovisno o scenariju iznad, slijedite korake u nastavku.

1. U pregledniku idite na http://manage.windowsazure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **NOVO** > **MREŽNIM servisima** > **VIRTUALNE MREŽE** > **STVORITE PRILAGOĐENI** kao što je prikazano na slici u nastavku.

    ![Stvaranje VNet portalu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Na stranici **Virtualne mreže pojedinosti** unesite **naziv** u VNet, odaberite **mjesto**, a zatim strelicu u donjem desnom kutu na stranici da biste prešli na stranici korak 2. Na slici u nastavku prikazuje postavke za naše scenarij.

    ![Stranica s detaljima virtualne mreže](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Na stranici **DNS poslužitelji i grupa podaci** , navedite naziv i IP adresa za do 9 DNS poslužitelji za korištenje. Ako ne navedete DNS poslužitelj, vaš VNet će koristiti Interna imenovanja razlučivost razlučivost nudi Azure. Za naše scenarij smo neće konfigurirati DNS poslužitelji.
5. Ako želite omogućuju pristup VPN točke na mjesto na VNet omogućiti potvrdni okvir **Konfiguriranje točke web VPN-a** . Ako ne konfigurirate točke web VPN-a, možete ga dodati u vašem VNet u bilo kojem trenutku nakon stvaranja. Za naše scenarij smo neće konfigurirati točke web VPN-a.
6. Ako želite omogućiti web-mjesto VPN veze između sustava VNet i drugi VNet ili lokalne mreže omogućite potvrdni okvir **Konfiguriranje VPN-a web-mjesto** i navedite želite li koristiti **ExpressRoute** ili bilješke, a naziv mreže povezati. Ako ne konfigurirate VPN-a web-mjesto, možete ga dodati u vašem VNet u bilo kojem trenutku nakon stvaranja. Za naše scenarij smo neće konfigurirati web-mjesto VPN-a.
7. Kliknite strelicu na desnoj donjem kutu stranice da biste prešli na korak 3. na slici u nastavku prikazuje postavke za naše scenarij.

    ![DNS poslužitelji i VPN povezivanja stranice](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Na stranici **Virtualne mreže adresu razmake** , u odjeljku **Pokretanje IP**na *10.0.0.0* da biste promijenili adresu prostor VNet kliknite, a zatim upišite početni adresnog prostora koji želite koristiti. Za naše scenarij upišite *192.168.0.0*. 
9. U odjeljku **CIDR (ADRESU zbroj)** odaberite broj bitova za masku podmreže. Za naše scenarij odaberite *16 (65536)*.
10. U odjeljku **PODMREŽE**kliknite *podmreže 1* i preimenovati podmreži ako je potrebno. Za naše scenarij promijenite naziv u *sučelju*.

    >[AZURE.NOTE] Ako kliknete izvan tekstni okvir naziv za podmreži će nećete moći uređivati naziv ako ponovno podmreži. Da biste riješili problem koji, morate ukloniti podmreži pritiskom na gumb sa znakom X s njezine desne strane, a zatim dodajte novi podmreže kao što je opisano u koraku 13 ispod.

11. U odjeljku **Pokretanje IP** za prvi podmreži navedite početni IP adresa za podmreži. Za naše scenarij upišite *192.168.1.0*.
12. U odjeljku **CIDR (ADRESU zbroj)** odaberite broj bitova za masku za prvi podmreži. Za naše scenarij odaberite *24 (256)*.
13. Ako je potrebno, kliknite da biste dodali novi podmreže, **dodajte podmreže** . Za naše scenarij dodajte podmreži, a zatim ponovite korake od 10 do 12 da biste konfigurirali VNet kao što je prikazano na slici u nastavku.

    ![Virtualne mreže adresu razmake stranice](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Kliknite gumb kvačica u desnom donjem kutu stranice da biste stvorili na VNet. Nakon nekoliko sekundi vašeg VNet prikazat će se na popisu dostupnih VNets, kao što je prikazano na slici u nastavku.

    ![Novi virtualne mreže](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)