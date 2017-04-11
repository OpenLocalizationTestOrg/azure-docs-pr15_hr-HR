|    | **Pristupnik za VPN propusnost (1)** | **Pristupnik za VPN Maks IPsec tunnels (2)** | **ExpressRoute pristupnika propusnost** | **Pristupnik za VPN-a i ExpressRoute supostojanje**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Osnovni SKU-om (3)(5)**              |  100 MB/s | 10                         |  500 MB/s                           | ne   |
| **Standardni SKU-om (4)(5)**           |  100 MB/s | 10                         | 1000 Mb/s                           | Da  |
| **Visoke performanse SKU (4)**   | 200 MB/s  | 30                         | 2000 MB/s                           | Da  |

- (1) propusnost za VPN je gruba Procjena temelju mjere između VNets u istom Azure regiji. Nije sigurno propusnost za više lokacija veze s Interneta. To je mjera Maksimalna moguće propusnost.
- (2) broj tunnels odnose se na RouteBased VPN-ovima. PolicyBased VPN-a podržavaju samo jedan tunelom VPN-a web-mjesto.
- (3) BGP nije podržana za osnovni SKU-om.
- (4) za ovaj SKU VPN-ovima PolicyBased nisu podržane. Podržani su za osnovni SKU samo.
- (5) za ovaj SKU veze aktivno aktivan S2S VPN pristupnika nisu podržane. Aktivni aktivno podržana za SKU HighPerformance na samo.