### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP podržana za sve Azure VPN pristupnika SKU-ove?

Ne, BGP podržana za Azure **standardnih** i **HighPerformance** VPN pristupnik. **Osnovni** SKU nije podržana.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Možete koristiti BGP s pristupnika Azure Policy-Based VPN-a?

Ne, BGP podržana za usmjeravanje sustavom VPN pristupnika samo.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Mogu li koristiti privatne ASNs (brojevi Autonomna sustava)?

Da, možete koristiti vlastite ASNs javne i privatne ASNs za vaše lokalne mreže i Azure virtualne mreže.

#### <a name="are-there-asns-reserved-by-azure"></a>Postoje li ASNs rezervirana Azure?

Da, sljedeće ASNs su rezervirana Azure za interne i vanjske peerings:

- Javni ASNs: 8075, 8076, 12076
- Privatna ASNs: 65515, 65517, 65518, 65519, 65520

Ne možete navesti te ASNs na lokalnom VPN uređaje prilikom povezivanja s Azure VPN pristupnika.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Mogu li koristiti isti ASN za lokalni VPN mreža i Azure VNets?

Ne morate dodijeliti različite ASNs između lokalnog mreža i vaše Azure VNets ako se povezujete ih zajedno s BGP. Azure VPN pristupnika imati zadani ASN 65515 dodijeljeni BGP je omogućen za ne stanje veze na više lokacija. Možete zaobići to je zadana vrijednost dodjeljivanjem različitih ASN prilikom stvaranja pristupnika VPN-a ili promijeniti u ASN nakon stvaranja pristupnika. Morat ćete vaše lokalne ASNs dodijelili odgovarajuće Azure lokalne mreže pristupnika.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Koje prefiksi adresa će Azure VPN pristupnika Oglasite mene?

Azure VPN pristupnika će Oglasite sljedeće usmjerava na lokalni BGP uređajima:

- Vaš prefiksi VNet adresa
- Prefiksi adresa za svaki lokalne mreže pristupnika povezani pristupnik Azure VPN-a
- Usmjerava naučili iz druge sesije BGP peering povezani pristupnik za Azure VPN **osim zadani smjer ili usmjerava preklapa s bilo kojeg VNet prefiks**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Mogu li oglašavanje zadani smjer (0.0.0.0/0) za Azure VPN pristupnika?

Zasad ne.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Li oglašavanje točno prefiksi kao Moja prefiksi virtualne mreže?

Ne, oglašavanje isti prefiksi kao jedan od virtualne mreže prefiksi adresa će biti blokirane ili Filtrirano prema Azure platforme. No možete Oglasite prefiksom koji je nadskup imate unutar virtualne mreže. Ako, na primjer, virtualne mreže nije pomoću prostor 10.10.0.0/16 adresa i nije Oglasite 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Mogu li koristiti BGP s vezama Moja VNet VNet?

Da, možete koristiti BGP za više lokacija veze i VNet VNet veze.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Možete li miješati BGP s vezama koje nisu BGP za moj pristupnika Azure VPN?

Da, možete kombinirati BGP i koje nisu BGP veza za istu Azure VPN pristupnika.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Označava Azure VPN pristupnika podršku BGP prijenosa usmjeravanje?

Da, BGP prijenosa usmjeravanje je podržano, uz iznimku Azure VPN-a **ne** će pristupnika Oglasite je zadani usmjerava na druge BGP kolegama. Da biste omogućili prijenosa usmjeravanje preko više pristupnika Azure VPN, morate omogućiti BGP na sve Srednja VNet VNet veze.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Mogu imati više od jedne tunnels između Azure VPN pristupnika i moju lokalnu mrežu?

Da, možete uspostaviti više S2S VPN tunnels između pristupnik za Azure VPN i lokalne mreže. Uzmite u obzir te tunnels će se brojati protiv ukupan broj tunnels za vaše Azure VPN pristupnika. Ako, na primjer, ako imate dvije suvišnih tunnels između pristupnik za Azure VPN i jednu lokalnu mrežu, mogu zauzeti će 2 tunnels iz ukupni kvote za Azure VPN pristupnika (10 za standardnu) i 30 za HighPerformance.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Mogu imati više tunnels između dva VNets Azure s BGP?

Ne, suvišne tunnels između par virtualne mreže nisu podržani.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Mogu li koristiti BGP za S2S VPN u konfiguraciji koegzistencija ExpressRoute/S2S VPN-a?

Zasad ne.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Koju adresu Azure VPN pristupnik koristi za IP ravnopravnih članova BGP?

Pristupnik za Azure VPN će dodijeliti jedan IP adrese iz raspona GatewaySubnet definirali za virtualne mreže. Po zadanom je adresu drugi zadnji raspon. Ako, na primjer, ako je vaš GatewaySubnet 10.12.255.0/27, od 10.12.255.0 do 10.12.255.31, zatim BGP ravnopravnih članova IP adresa pristupnika Azure VPN bit će 10.12.255.30. Ove informacije možete pronaći kada popis pristupnika informacije Azure VPN-a.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Koji su preduvjeti za BGP ravnopravnih članova IP adrese na uređaju VPN-a?

Vaše lokalne BGP ravnopravni adresu **Ne mora** biti isti kao javnu IP adresu VPN uređaja. Korištenje drugu IP adresu na uređaju VPN-a za IP BGP ravnopravnih članova. Možda ćete adresu dodijeljene sučelja povratna na uređaju. Navedite tu adresu u odgovarajuće lokalne mreže pristupnika koji predstavlja mjesto.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Što se navedite kao prefiksi moje adrese za pristupnik za lokalnu mrežu koristite BGP?

Azure lokalne mreže pristupnika određuje prefiksi Početna adresa za lokalnu mrežu. S BGP, morate dodijeliti prefiks glavnog računala (/ 32 prefiks) BGP ravnopravnih članova IP adrese kao prostor adrese za taj lokalne mreže. Ako je IP ravnopravnih članova BGP 10.52.255.254, navedite "10.52.255.254/32" kao localNetworkAddressSpace lokalne mreže pristupnika koji predstavlja lokalne mreže. To je da biste bili sigurni pristupnika Azure VPN uspostavlja je sesiju BGP kroz tunelom S2S VPN-a.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Što dodavati na uređaj lokalnog VPN-a za sesiju peering BGP?

Glavno računalo rute Azure BGP ravnopravnih članova IP adrese trebali biste dodati na uređaju VPN koja pokazuje na tunelom IPsec S2S VPN-a. Ako, na primjer, ako je IP ravnopravnih članova za VPN Azure "10.12.255.30", trebali biste dodavanje rutu glavno računalo za "10.12.255.30" s nexthop sučelja koji se podudaraju sučelja tunelom IPsec na uređaju VPN-a.
