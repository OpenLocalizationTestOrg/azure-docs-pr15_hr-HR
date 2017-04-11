U sljedećoj su tablici navedeni preduvjeti za PolicyBased i RouteBased VPN pristupnik. U ovoj su tablici primjenjuje se na Voditelj resursa i uvođenje klasičnog modela. Klasični modela PolicyBased VPN pristupnika isti su kao statične pristupnika, a utemeljen na usmjeravanje pristupnika isti su kao i dinamičkih pristupnika.


|   | **Osnovni PolicyBased VPN pristupnika** | **Osnovni RouteBased VPN pristupnika** | **Standardna RouteBased VPN pristupnika**   | **RouteBased visoke performanse VPN pristupnika** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Povezivanje web-mjesto (S2S)**  | Konfiguriranje PolicyBased VPN-a        | Konfiguriranje RouteBased VPN-a  | Konfiguriranje RouteBased VPN-a     | Konfiguriranje RouteBased VPN-a    |
| Povezivanje **točke-na-web-mjesta (P2S**)      | Nije podržano   | Podržani (mogu postojati zajedno sa S2S)  | Podržani (mogu postojati zajedno sa S2S)  | Podržani (mogu postojati zajedno sa S2S) |
| **Način provjere autentičnosti**                 |    Zajednički ključ  | Zajednički ključ za povezivanje S2S certifikata za povezivanje P2S | Zajednički ključ za povezivanje S2S certifikata za povezivanje P2S | Zajednički ključ za povezivanje S2S certifikata za povezivanje P2S |
| **Maksimalni broj S2S veza**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maksimalni broj P2S veza**       | Nije podržano                  | 128                                                                   | 128                               | 128                              |
|**Aktivni podršku za usmjeravanje (BGP)**           | Nije podržano                  | Nije podržano                                                         | Podržana                     | Podržana                   |
 
