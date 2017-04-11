<properties
    pageTitle="Najčešća pitanja – Azure Active Directory Domain Services | Microsoft Azure"
    description="Najčešća pitanja o Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Najčešća pitanja

Ova stranica navedeni odgovori na najčešća pitanja o na Azure Active Directory Domain Services. Zadrži traženje natrag ažuriranja.

### <a name="troubleshooting-guide"></a>Vodič za otklanjanje poteškoća
Potražite naš [Vodič za otklanjanje poteškoća](active-directory-ds-troubleshooting.md) za rješenja uobičajenih problema do prilikom konfiguriranja ili administriranje Azure servisa Active Directory Domain Services.


### <a name="configuration"></a>Konfiguracija

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Je li moguće stvoriti više domena za pojedinačni Azure AD direktorija?
ne. Možete stvoriti samo jedan domene kvalificiranom Azure servisa Active Directory Domain Services za jednu Azure AD direktorija.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Možete omogućiti Azure AD domenske servise u Voditelj resursa Azure virtualne mreže?
ne. Azure servisa Active Directory Domain Services može se omogućiti samo u klasični Azure virtualne mreže. Klasični virtualne mreže možete povezati s resursima virtualne mreže pomoću virtualne mreže peering radije koristili domenu upravljanih resursima virtualne mreže.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Što učiniti Azure servisa Active Directory Domain Services dostupne u više mreža virtualne unutar pretplate?
Sam servis izravno ne podržavaju taj scenarij. Azure servisa Active Directory Domain Services dostupna je u virtualne mreže samo jedan po jedan. Međutim, možete konfigurirati veze između više virtualne mreže da biste otkrili Azure servisa Active Directory Domain Services s drugim mrežama virtualne. U ovom se članku opisuje kako možete [povezati virtualne mreže u Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Možete omogućiti servisa Azure AD domene pomoću komponente PowerShell?
PowerShell/automatski implementacije Azure servisa Active Directory Domain Services trenutno nije dostupna.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure servisa Active Directory Domain Services dostupna je u novom portalu Azure?
ne. Azure servisa Active Directory Domain Services može se konfigurirati samo u [Azure klasični portal](https://manage.windowsazure.com). Ne možemo očekivati da biste proširili podrška za [Azure portal](https://portal.azure.com) u budućnosti.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kako dodati kontrolera domena programa Azure servisa Active Directory Domain Services upravljanih prilagođenu domenu?
ne. Domena nudi Azure servisa Active Directory Domain Services je upravljanih domene. Morate Dodjela resursa, konfiguriranje i inače upravljanje kontrolera domena za tu domenu - ove aktivnosti upravljanja nudi kao servis Microsoft. Zbog toga ne možete dodati kontrolera dodatnu domenu (čitanje i pisanje ili samo za čitanje) za upravljane domenu.

### <a name="administration-and-operations"></a>Administracija i operacije

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Načini povezivanja s kontrolerom domene za upravljane domene putem udaljene radne površine?
ne. Nemate dozvolu za povezivanje s kontrolera domena za upravljane domene putem udaljene radne površine. Članovi grupe 'AAD Kontroler administratori' možete upravljati upravljanih domene pomoću alata za administraciju servisa Active Directory kao što je Active Directory Administracija centra (ADAC) ili AD PowerShell. Ti Alati se instaliraju pomoću značajke "Remote Server Administracija Tools" na poslužitelju Windows pridruženo upravljanih domeni.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Koje ste omogućili Azure servisa Active Directory Domain Services. Koji račun koristiti za domenu spoj strojeva za tu domenu?
Korisnicima koji ste dodali u grupu administratora (na primjer, "AAD Kontroler administratori') možete strojeva domene spoj. Uz to, korisnici u ovoj grupi su pristup udaljene radne površine na računalima koje je pridruženo domene.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Mogu li wield domene administratorske ovlasti za domenu koje ste dobili od Azure servisa Active Directory Domain Services?
ne. Ne dobijete administrativnim privilegijama za upravljane domene. Ovlasti ' Administrator domene ' i ' Administrator u tvrtki ' nisu dostupne za korištenje u domeni. Postojeće administrator domene ili grupe administratora enterprise unutar direktorija Azure AD i ne odobravaju domene/enterprise administratorske ovlasti na domeni.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Možete promijeniti pomoću LDAP ili druge AD Administrativni alati na domena nudi Azure servisa Active Directory Domain Services članstva u grupi?
ne. Članstva u grupi nije moguće mijenjati na domena kvalificiranom Azure servisa Active Directory Domain Services. Isto vrijedi za korisničke atribute. Možda ipak promijenite članstva u grupi ili korisničke atribute u Azure AD ili na lokalne domene. Izmjena automatski se sinkroniziraju Azure servisa Active Directory Domain Services.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Mogu proširiti sheme domene nudi Azure servisa Active Directory Domain Services?
ne. Microsoft upravlja shema za upravljane domenu. Azure servisa Active Directory Domain Services ne podržava proširenja shema.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Možete li se izmijeniti ili dodajte DNS zapise u upravljanih domene?
Da. Korisnici koji pripadaju grupi 'AAD Kontroler administratori' odobravaju ovlasti "DNS Administrator" da biste izmijenili DNS zapise u domeni upravljanih. Ti korisnici možete koristiti na konzoli upravitelja DNS-om na računalu sa sustavom Windows Server priključeno upravljanih domenu za upravljanje DNS-a. Da biste koristili konzole Upravitelja DNS-om, instalirajte 'DNS poslužitelja Alati", koja je dio"Remote Server Administracija Tools"dodatna značajka na poslužitelju. Dodatne informacije o [uslužni programi za administriranje, praćenje i rješavanje problema DNS](https://technet.microsoft.com/library/cc753579.aspx) dostupna je na mreži TechNet.


### <a name="billing-and-availability"></a>Naplata i dostupnosti

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Je li Azure AD domene servisa plaćenu servis?
Da. Dodatne informacije potražite u članku [cijene stranice](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Je li besplatnu probnu verziju servisa?
Taj servis je sve obuhvaćeno besplatnu probnu verziju za Azure. Možete se prijaviti za [besplatnu probnu verziju jedan mjesec Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Mogu li dobiti Azure AD domenske servise kao dio sustava paket Enterprise mobilnost (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Moram li Azure AD Premium korištenja servisa Azure AD domene?
ne. Azure servisa Active Directory Domain Services je pay-as-you-go servisa Azure i nije dio EMS. Azure servisa Active Directory Domain Services dostupne su za sve izdanja Azure AD (besplatno, Basic i, Premium) i se naplatiti na temelju svaki sat, ovisno o korištenju.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Što Azure područja servis dostupna je u?
Odnose se na stranici [Servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) da biste vidjeli popis Azure područja u kojem je dostupna Azure servisa Active Directory Domain Services.
