<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Najčešća pitanja vezana uz - obrnutim DNS-om za Azure dodijeljena IP adrese

### <a name="how-much-do-reverse-dns-records-cost"></a>Koliko obrnuti trošak DNS zapisa?
Slobodno se!  Postoji bez dodatnih troškova za obrnutom DNS zapise ili upita.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Će riješiti za obrnutim DNS zapisi s Interneta
Da. Kada postavite svojstvo obrnutom DNS-a za servis u Oblaku, Azure upravlja DNS-a delegations i DNS zone potrebne da biste bili sigurni da razrješava obrnutom DNS zapis za sve korisnike internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Zadani obrnutim DNS zapisa koji se stvara za servise u Oblaku?
ne. Obrnuti DNS je značajka izborno. Nema zadani obrnutim DNS zapisi se stvaraju Ako odlučite da ne želite ih konfigurirali.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Što je oblik u potpunosti puni naziv domene (FQDN)?
Certifikatom određeni su klauzulom prosljeđivanje redoslijedom, a morate prekinuta točka (npr., "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Što se događa ako provjere valjanosti za obrnutom DNS koje ste naveli neće funkcionirati?
Gdje se provjerava provjere valjanosti za obrnutim DNS neće funkcionirati, postupak upravljanja usluge neće uspjeti. Ispravljanje obrnutim DNS vrijednosti po potrebi i pokušajte ponovno.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Možete upravljati obrnutim DNS-a za Moje web-mjesto Azure?
Obrnuti DNS nije podržana za Azure web-mjesta. Obrnuti DNS nije podržana za Azure PaaS uloge i IaaS virtualnim računalima.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Ću konfigurirati više obrnutim DNS zapise za servis u Oblaku?
ne. Azure podržava jedan obrnutim DNS zapis za svaku uslugu Azure oblaka. Svaki servis u Oblaku Azure no može imati vlastite obrnutim DNS zapis.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Mogu li slati poruke e-pošte vanjskih domena sa servisima za izračun Azure?
ne. [Izračunati azure services ne podržavaju slanje poruke e-pošte da biste vanjskih domena](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
