## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Kako stvoriti VNet pomoću datoteke config mreže na portalu za Azure

Azure koristi xml datoteku da biste definirali VNets sve dostupne na pretplatu. Možete preuzeti ove datoteke i uređivati možete mijenjati ili brisati postojeće VNets i stvarati nove. U ovom dokumentu će upute za preuzimanje datoteke, se nazivaju mrežne datoteke koje konfiguracije (ili netcgf) i uređivanje da biste stvorili novi VNet. Provjerite [sheme konfiguracije Azure virtualne mreže](https://msdn.microsoft.com/library/azure/jj157100.aspx) da biste saznali više o datoteci konfiguracije mreže.

Da biste stvorili VNet pomoću datoteke netcfg putem portala za Azure, slijedite korake u nastavku.

1. U pregledniku idite na http://manage.windowsazure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Pomaknite se prema dolje na popisu servisa pa kliknite na **MREŽAMA** , kao što se vidi ispod.

    ![Azure virtualne mreže](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Pri dnu stranice, kliknite gumb **IZVEZI** kao što je prikazano u nastavku.

    ![Gumb Izvezi](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Na stranici **Izvoz mrežna konfiguracija** odaberite pretplatu u koju želite izvesti Konfiguracija virtualne mreže s, a zatim gumb kvačicu na lijevoj strani donjem kutu stranice.
5. Slijedite upute za preglednik da biste spremili datoteku **NetworkConfig.xml** . Provjerite nalaze li se obratite pažnju na koje spremate datoteku.
6. Otvorite datoteku koju ste spremili u koraku 5 pomoću bilo koju aplikaciju uređivač XML-a ili tekst, a zatim potražite na **<VirtualNetworkSites>** element. Ako imate neki mreža već stvorili, svaku prikazat će se kao vlastitu **<VirtualNetworkSite>** element.
7. Da biste stvorili virtualne mreže opisani u ovom scenariju, dodajte sljedeće XML samo u odjeljku s **<VirtualNetworkSites>** elementa:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Spremite datoteku konfiguracije mreže.
9.  Na portalu Azure, na lijevoj strani donjem kutu stranice, kliknite **NOVO**, a zatim kliknite **MREŽNIM servisima**, a zatim kliknite **VIRTUALNE MREŽE**, a zatim **UVOZ KONFIGURACIJE** kao što je prikazano na slici u nastavku.

    ![Uvoz konfiguracije](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Na stranici **Uvoz konfiguracijska datoteka mreže** kliknite **PREGLEDAJ za datoteke...**, a zatim dođite do mape u koje ste spremili datoteku u koraku 8, odaberite datoteku i kliknite **Otvori**. Web-stranici trebala bi izgledati slično kao na slici u nastavku. Na u donjem desnom kutu stranice, kliknite gumb sa strelicom da biste premjestili na sljedeći korak.

    ![Uvoz mreže konfiguracijska datoteka stranica](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Na stranici **Stvaranje mreže** obratite pozornost na stavci za svoje nove VNet kao što je prikazano na slici u nastavku.

    ![Stvaranje stranice mreže](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Kliknite gumb kvačica u desnom donjem kutu stranice da biste stvorili na VNet. Nakon nekoliko sekundi vašeg VNet prikazat će se na popisu dostupnih VNets, kao što je prikazano na slici u nastavku.

    ![Novi virtualne mreže](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)