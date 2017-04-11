## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Kako stvoriti klasični VNet na portalu za Azure

Da biste stvorili klasični VNet ovisno o scenariju iznad, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **NOVO** > **umrežavanje** > **virtualne mreže**, obavijest već prikazuje **Klasični**na popisu **Odaberite model implementacije** , a zatim kliknite **Stvori**, kao što se vidi na slici u nastavku.

    ![Stvaranje VNet portalu za Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Na plohu **virtualne mreže** unesite **naziv** u VNet, a zatim **prostor adrese**. Konfigurirati postavke adresu prostora na VNet i njegov prvi podmreže, a zatim kliknite **u redu**. Na slici u nastavku prikazuje postavke blokiranja CIDR za naše scenarij.

    ![Adresa prostora plohu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Kliknite **Grupu resursa** i odaberite grupu resursa da biste dodali VNet da biste ili kliknite **Stvori novu grupu resursa** u VNet dodati novu grupu resursa. Na slici u nastavku prikazuje resursa postavki grupe za novu grupu resursa pod nazivom **TestRG**. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Stvaranje plohu grupa resursa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Ako je potrebno, promijenite postavke **pretplate** i **mjesto** za vaše VNet. 

6. Ako ne želite vidjeti na VNet kao pločicu u **Startboard**, onemogućite **Prikvači na Startboard**. 

7. Kliknite **Stvori** i obratite pozornost na pločici pod nazivom **Stvaranje virtualne mreže** , kao što je prikazano na slici u nastavku.

    ![Stvaranje VNet portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Pričekajte VNet će biti stvoren, a kada se prikaže pločicu ispod, kliknite da biste dodali više podmreže.

    ![Stvaranje VNet portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Trebali biste vidjeti **konfiguracije** za vaše VNet, kao što je prikazano u nastavku. 

    ![Stvaranje VNet portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Kliknite **podmreže** > **Dodavanje**, zatim upišite **naziv** i navedite **adresu raspona (CIDR bloka)** za vašoj podmreži, a zatim kliknite **u redu**. Na slici u nastavku prikazuje postavke za naše trenutnog scenarija.

    ![Stvaranje VNet portalu za Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)