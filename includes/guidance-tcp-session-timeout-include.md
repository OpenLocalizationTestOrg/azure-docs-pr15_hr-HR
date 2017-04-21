##<a name="tcp-settings-for-azure-vms"></a>TCP postavke za Azure VMs

Azure VMs komunikaciju s javnog interneta pomoću [NAT] [ nat] (prevođenja mrežnih adresa). Uređaji za NAT dodijeliti javnu IP adresa i priključaka VM programa Azure koji omogućuje VM uspostaviti socket za komunikaciju s drugim uređajima. Ako pakete prestali slijedi do tog socket nakon određenog vremena, uređaj NAT kills mapiranje i na socket je besplatno se može koristiti u drugim VMs.

Ovo je uobičajenih ponašanje NAT koje mogu uzrokovati probleme komunikacije aplikacije TCP temelji očekivana socket zapisnik nakon isteka razdoblja. Postoje dvije postavke neaktivnosti vremenskog ograničenja treba uzeti u obzir za sesije u stanju *uspostaviti vezu* :

- **dolazni** kroz [Azure učitavanje raspoređivača][azure-lb-timeout]. Ovaj vremenskog ograničenja zadanih postavki 4 minuta, a zatim je moguće prilagoditi prema gore za 30 minuta.
- **Izlazni** pomoću [SNAT] [ snat] (izvor NAT). U ovom vremenskog ograničenja postavljena na 4 minuta, a nije moguće prilagoditi.

Da biste osigurali veza se prekinula izvan roku, provjerite je li aplikacija zadržava sesiju aktivnosti ili možete konfigurirati Temeljni operacijski sustav da biste to učinili. Postavke koje se koriste se razlikuju za Linux i Windows sustave, kao što je prikazano u nastavku.

Za [Linux][linux], trebali biste promijeniti otklanjanje varijable u nastavku.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Za [Windows][windows], promijenite vrijednosti registra.
KeepAliveInterval i = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Postavke iznad provjerite je li zadržati aktivnosti paket šalje nakon dvije minute (u sekundama 120) vrijeme neaktivnosti, i šalju svakih 30 sekundi. A ako 8 te pakete uspjeti, sesiju prekida.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md