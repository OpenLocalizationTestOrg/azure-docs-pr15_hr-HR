## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Uvođenje predloška ARM pomoću kliknite radi implementacije

Možete ponovno korištenje unaprijed definiranih ARM predlošci prijenos github spremište održava tvrtka Microsoft i otvoriti na zajednicu. Te predloške možete uvesti izravno iz github, ili preuzeti i izmijeniti da odgovara vašim potrebama. Da biste implementirali predložak koji stvara u VNet s dva podmreže, slijedite korake u nastavku.

1. U pregledniku dođite do [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Pomaknite se prema dolje na popisu predložaka pa kliknite **101-vnet-dva-podmreže**. Provjerite datoteke **README.md** , kao što je prikazano u nastavku.

    ![Datoteke READEME.md u github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Kliknite **Implementacija Azure**. Ako je potrebno, unesite vjerodajnice za prijavu Azure. 
4. U plohu **parametara** unesite vrijednosti koje želite koristiti za stvaranje svoje nove VNet, a zatim **u redu**. Na slici u nastavku prikazuje vrijednosti za naše scenarij.

    ![Parametri predložaka ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Kliknite **grupu resursa** i odaberite grupu resursa da biste dodali VNet da biste ili kliknite **Stvori novi** da biste dodali u VNet u novu grupu resursa. Na slici u nastavku prikazuje resursa postavki grupe za novu grupu resursa pod nazivom **TestRG**.

    ![Grupa resursa](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Ako je potrebno, promijenite postavke **pretplate** i **mjesto** za vaše VNet.
6. Ako ne želite vidjeti na VNet kao pločicu u **Startboard**, onemogućite **Prikvači na Startboard**.
5. Kliknite **Leagl uvjete**, uvjeti i kliknite **Kupi** da biste slažete. 
6. Kliknite **Stvori** da biste stvorili na VNet.

    ![Slanja implementacije pločica u portal (pretpregled)](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Kada završi implementaciju, kliknite **TestVNet** > **sve postavke** > **podmreže** da biste pogledali svojstva podmreže, kao što je prikazano u nastavku.

    ![Stvaranje VNet u portal (pretpregled)](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)