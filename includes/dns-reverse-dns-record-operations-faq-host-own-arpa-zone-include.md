<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Najčešća pitanja vezana uz - Hosting svoju ARPA zonu DNS-a za Azure

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Možete li hostirati ARPA zone za Moj ISP dodijeljeni IP blokova na Azure DNS?
Da. Hostiranje ARPA zone za vlastitu IP rasponi u Azure DNS potpuno podržane.

Jednostavno [Stvaranje zone u Azure DNS](dns-getstarted-create-dnszone.md), a zatim rad sa svog davatelja internetskih usluga delegatu [zone](dns-domain-delegation.md).  Zatim možete upravljati PTR zapisi za svaku obrnutom pretraživanja na isti način kao i druge vrste zapisa.

Možete i [uvesti postojeće obrnuto pretraživanje zone pomoću EŽA Azure](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Koliko stoji hosting Moje trošak zone ARPA?
Hosting ARPA zone IP ISP dodijeljeni bloka u Azure DNS se naplaćuje na [standardni Azure DNS stope](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Možete li se hostirati ARPA zone za IPv4 i IPv6 adrese u Azure DNS?
Da.