## <a name="download-and-understand-the-arm-template"></a>Preuzimanje i razumijevanje predložak ARM

Možete preuzeti postojećeg predloška OKVIRA za stvaranje na VNet i dva podmreže iz github, unesite željene promjene možda, a zatim ga ponovno upotrijebiti. Da biste to učinili, slijedite korake u nastavku.

1. Idite na [stranicu predloška uzorka](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Kliknite **azuredeploy.json**, a zatim **RAW**.
3. Spremite datoteku u lokalnoj mapi na računalu.
4. Ako ste upoznati s predlošcima ARM, preskočite do koraka 7.
5. Otvorite datoteku koju ste upravo spremili i pregled sadržaja u odjeljku **parametara** u retku 5. Parametri predložaka ARM navedite rezervirano mjesto za vrijednosti koje je moguće ispuniti tijekom implementacije.

    | Parametar | Opis |
    |---|---|
    | **mjesto** | Azure područje u kojem će se stvoriti u VNet |
    | **vnetName** | Naziv za novu VNet |
    | **addressPrefix** | Adresa prostora za VNet u obliku CIDR |
    | **subnet1Name** | Naziv prve VNet |
    | **subnet1Prefix** | CIDR Blok za prvu podmreže |
    | **subnet2Name** | Naziv drugog VNet |
    | **subnet2Prefix** | CIDR Blok za drugi podmreže |

    >[AZURE.IMPORTANT] Predlošci ARM zajamčena github možete promijeniti tijekom vremena. Provjerite jeste li predložak prije korištenja.
    
6. Provjerite sadržaj pod **resurse** i Primijetit ćete sljedeće:

    - **Vrsta**. Vrsta resursa stvoren u predlošku. U ovom slučaju **Microsoft.Network/virtualNetworks**koji predstavljaju na VNet.
    - **naziv**. Naziv resursa. Obratite pozornost na korištenje **[parameters('vnetName')]**, što znači da će naziv pruža kao ulaz korisnika ili datoteku parametar tijekom implementacije.
    - **Svojstva**. Popis svojstava za resurs. Ovaj predložak koristi svojstva prostor i podmreže adrese tijekom stvaranja VNet.

7. Otvorite [oglednu stranicu predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Kliknite **azuredeploy paremeters.json**, a zatim **RAW**.
9. Spremite datoteku u lokalnoj mapi na računalu.
10. Otvorite datoteku koju ste upravo spremili i uređivati vrijednosti za parametre. Pomoću sljedećih vrijednosti za implementaciju VNet opisane u našem scenarij.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Spremite datoteku.
  