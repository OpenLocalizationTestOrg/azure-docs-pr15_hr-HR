## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Kako stvoriti na VNet na portalu za Azure

Da biste stvorili VNet temelju scenarij iznad pomoću portala za Azure pretpregled, slijedite korake u nastavku.

1. U pregledniku idite na http://portal.azure.com i, ako je potrebno, prijavite se pomoću računa za Azure.
2. Kliknite **NOVO** > **umrežavanje** > **virtualne mreže**, zatim kliknite **Upravitelj resursa** s popisa **Odaberite model implementacije** , a zatim **Stvori**, kao što se vidi na slici u nastavku.

    ![Stvaranje VNet portalu za Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Na plohu **Stvaranje virtualne mreže** konfigurirati postavke VNet kao što je prikazano na slici u nastavku.

    ![Stvaranje plohu virtualne mreže](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Kliknite **grupu resursa** i odaberite grupu resursa da biste dodali VNet da biste ili kliknite **Stvori novi** da biste dodali u VNet u novu grupu resursa. Na slici u nastavku prikazuje resursa postavki grupe za novu grupu resursa pod nazivom **TestRG**. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/resource-group-overview.md#resource-groups).

    ![Grupa resursa](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Ako je potrebno, promijenite postavke **pretplate** i **mjesto** za vaše VNet. 

6. Ako ne želite vidjeti na VNet kao pločicu u **Startboard**, onemogućite **Prikvači na Startboard**. 

7. Kliknite **Stvori** i obratite pozornost na pločici pod nazivom **Stvaranje virtualne mreže** , kao što je prikazano na slici u nastavku.

    ![Stvaranje virtualne mreže pločica](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Pričekajte da VNet će biti stvoren, a zatim u plohu **virtualne mreže** **sve postavke** > **podmreže** > **Dodaj** kao što se vidi ispod.

    ![Dodavanje podmreže na portalu za Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Odredite postavke podmreže podmreže *pozadinskog* kao što je prikazano u nastavku, a zatim kliknite **u redu**. 

    ![Postavke podmreži](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Obratite pozornost na popisu podmreže, kao što je prikazano na slici u nastavku.

    ![Popis podmreže u VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
