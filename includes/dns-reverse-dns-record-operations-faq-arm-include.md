<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Najčešća pitanja vezana uz - obrnutim DNS-om za Azure dodijeljena IP adrese

### <a name="how-much-do-reverse-dns-records-cost"></a>Koliko obrnuti trošak DNS zapisa?
Slobodno se!  Postoji bez dodatnih troškova za obrnutom DNS zapise ili upita.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Će obrnutim DNS zapise za Azure dodijeljeni javnu IP adresu e-riješiti problem s Internetom?
Da. Nakon postavljanja svojstvo obrnutim DNS-a za javnu IP adresu Azure upravlja DNS delegations i DNS zone potrebno da biste bili sigurni da razrješava obrnutim DNS zapis za sve korisnike internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Zadani obrnutim DNS zapisa za stvorit će se javnu IP adresama?
ne. Obrnuti DNS je značajka izborno. Nema zadani obrnutim DNS zapisi se stvaraju Ako odlučite da ne želite ih konfigurirali.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Što je oblik u potpunosti puni naziv domene (FQDN)?
Certifikatom određeni su klauzulom prosljeđivanje redoslijedom, a morate prekinuta točka (npr., "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Što se događa ako provjere valjanosti za obrnutom DNS koje ste naveli neće funkcionirati?
Gdje se provjerava provjere valjanosti za obrnutim DNS neće funkcionirati, postupak upravljanja usluge neće uspjeti. Ispravljanje obrnutim DNS vrijednosti po potrebi i pokušajte ponovno.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Možete upravljati obrnutim DNS-a za Moje web-mjesto Azure?
Obrnuti DNS nije podržana za Azure web-mjesta. Obrnutom DNS podržana za Azure virtualnim računalima.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Ću konfigurirati više obrnutim DNS zapise za javnu IP adresu?
ne. Azure podržava jedan obrnutom DNS zapis za svaku javnu IP adresu. Svaki javnu IP adresu no može imati vlastite obrnutim DNS zapis.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Ću konfigurirati obrnutim DNS zapise za IPv6 javnu IP adresa?
ne.  Trenutačno obrnutim DNS zapise za podržane su IPv4 javnu IP adrese samo.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Možete ću konfigurirati obrnutim DNS zapis za javnu IP adresu bez DomainNameLabel navedeni?
ne. Korištenje obrnutom DNS zapise za javnu IP adrese, morate navesti svojstvo DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Mogu li slati poruke e-pošte vanjskih domena sa servisima za izračun Azure?
ne. [Izračunati azure services ne podržavaju slanje poruke e-pošte da biste vanjskih domena](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
