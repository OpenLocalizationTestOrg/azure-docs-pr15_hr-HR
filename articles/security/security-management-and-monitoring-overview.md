<properties
   pageTitle="Upravljanje Azure sigurnost i nadzor pregled | Microsoft Azure"
   description=" Azure sadrži sigurnosne mehanizme da biste olakšali upravljanje i nadzor servise u oblaku Azure i virtualnih računala.  Ovaj članak sadrži pregled tih core sigurnosnim značajkama i usluge. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Upravljanje Azure sigurnost i nadzor pregled

Azure sadrži sigurnosne mehanizme da biste olakšali upravljanje i nadzor servise u oblaku Azure i virtualnih računala. Ovaj članak sadrži pregled tih core sigurnosnim značajkama i usluge. Veza se daju na članke koji steći pojedinosti svake da biste saznali više.

Sigurnost servisa Microsoft cloud je partnerstvo i zajedničke odgovornosti između vas i tvrtke Microsoft. Zajedničke odgovornosti označava Microsoft je zadužen za Microsoft Azure i fizičke sigurnost podataka pogoni (pomoću zaštiti sigurnosti kao što su zaključane iskaznice stavka vrata, fences i guards). Osim toga, Azure nudi istaknuti razine sigurnosti oblaka sloju softver koji odgovara potrebama sigurnost, privatnost i usklađenost njegov zahtjevne klijenata.

Vi ste vlasnik podataka i identiteta, odgovornost za zaštitu ih, sigurnost lokalnog resursa i sigurnost oblaka komponente pokazivač koji vam omogućuje kontrolu. Microsoft pruža mogućnosti da biste zaštitili podataka i aplikacije i sigurnosne kontrole. Odgovornost za sigurnost temelji se na vrsti servis u oblaku.

Sljedeći grafikon prikazuje saldo odgovornost za Microsoft te kupca.

![Zajedničke odgovornosti][1]

Detaljnije dive u sigurnost upravljanje potražite u članku [Upravljanje sigurnost u Azure](azure-security-management.md).

Ovo su osnovne funkcije da biste se prekriveno u ovom članku:

- Kontrola pristupa na temelju uloga
- Antimalware
- Višestruka provjera autentičnosti
- ExpressRoute
- Virtualne mreže pristupnika
- Upravljanje identitetom povlaštene
- Zaštita identiteta
- Centar za sigurnost

## <a name="role-based-access-control"></a>Kontrola pristupa na temelju uloga

Na temelju uloga kontrole pristupa (RBAC) pruža mogućnost upravljanja preciznije pristup za Azure resurse. Pomoću RBAC možete dodijeliti osobama iznos programa access koje su im potrebne za izvođenje svoje zadatke.  RBAC mogu pomoći utvrditi da kada korisnici ostavite tvrtke ili ustanove mogu izgubiti pristup resursima u oblaku.

uči više:

- [Active Directory bloga tima na RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Kontrola Azure pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

S Azure možete koristiti softver antimalware dobavljača glavne sigurnost kao što je Microsoft, Symantec, Trend Micro, tvrtke McAfee i Kaspersky da biste zaštitili virtualnih računala od štetnih datoteke, reklamni i drugih prijetnji.

Microsoft Antimalware nudi mogućnost da biste instalirali antimalware agent za PaaS uloge i virtualnih računala. Na temelju Zaštita na krajnjoj točki centar sustava značajka poziva dokazana lokalnog sigurnosti tehnologije u oblak.

Nudimo precizno integracije za Trend- [Precizno sigurnost](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ i [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ proizvode Azure platforme. DeepSecurity je antivirusni rješenje i SecureCloud je rješenje za šifriranje. DeepSecurity će biti implementirano unutar VMs pomoću programa proširenje modela. Pomoću portala za korisničko Sučelje i PowerShell, možete odabrati da biste koristili DeepSecurity unutar nove VMs koji su se spun prema gore ili postojeći VMs koji su već ste implementirali.

Zaštita Symantec krajnju točku (SEP) podržano je i na Azure. Portala integracijom korisnici imati mogućnost da biste odredili ih namjeravate koristiti SEP unutar na VM. SEP moguće je instalirati na potpuno novi VM putem portala za Azure ili moguće je instalirati na do postojeće VM pomoću komponente PowerShell.

uči više:

- [Implementacija rješenja Antimalware na Azure virtualnim strojevima](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Antimalware za servise u Oblaku Azure i virtualnih računala](../security/azure-security-antimalware.md)
- [Kako instalirati i konfigurirati Trend Micro precizno sigurnost kao servisa na VM za Windows](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Kako instalirati i konfigurirati Zaštita na krajnjoj točki Symantec na VM za Windows](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nove mogućnosti Antimalware za zaštitu virtualnim strojevima Azure – Zaštita na krajnjoj točki tvrtke McAfee](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Višestruka provjera autentičnosti

Azure višestruke provjere autentičnosti (MFA) je metoda provjere autentičnosti koja zahtijeva korištenje više način potvrde i dodaje ključnih drugu razinu zaštite znak dodaci za korisnika i transakcije. MFA olakšava zaštitu programa access s podacima i aplikacijama tijekom sastanka korisnika zahtjev za jednostavne postupak prijave. On nudi Jaka provjera autentičnosti putem raspon mogućnosti provjere – telefonski poziv, tekstne poruke ili mobilne aplikacije obavijesti ili potvrdu Šifra i 3 strana OATH tokeni.

uči više:

- [Višestruka provjera autentičnosti](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Što je Azure višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md)
- [Kako funkcionira Azure višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute omogućuje širenje vaše lokalne mreže Microsoft cloud putem namjenski privatne veze olakšano davatelj povezivanje. S ExpressRoute, možete uspostaviti veze s Microsoftovim servisima u oblaku, kao što je Microsoft Azure, Office 365 i CRM Online. Povezivanje može biti iz programa bilo koje-na-bilo koje mreže (VPN-a IP), point-to-point Ethernet mreže ili virtualne unakrsno-vezu putem davatelja povezivanja na funkciju zajednički mjesto. ExpressRoute veze ne otvorite putem javnog Interneta. Time se omogućuje ExpressRoute veze ponuditi dodatne pouzdanosti, brže brzine, donjem latencies i veću sigurnost od standardne veze putem Interneta.

uči više:

- [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtualne mreže pristupnika

Pristupnika VPN-a naziva se i Azure virtualne mreže pristupnicima koriste se za slanje mrežni promet između virtualne mreže i lokalne lokacije. Također se koriste za slanje promet između više mreža virtualne unutar Azure (VNet-VNet).  VPN pristupnika omogućuju sigurno više lokacija veze između Azure i preduvjete infrastrukture.

uči više:

- [O pristupnika VPN-a](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Pregled sigurnosti Azure mreže](security-network-overview.md)

## <a name="privileged-identity-management"></a>Upravljanje identitetom povlaštene

Katkad korisnici morati izvršavanje povlaštene operacije u Azure resursa ili drugih aplikacija SaaS. To obično znači tvrtke ili ustanove da biste im dodijelili trajna povlaštene pristup servisa Azure Active Directory (Azure AD). To je sve veći sigurnosni rizik za oblak hostira resurse jer tvrtkama ili ustanovama potpuno nije moguće nadzirati što ti korisnici rade s njihov povlaštene pristup.
Uz to, korisnički račun s pristupom povlaštene ugrožena, taj jedan kršenja mogli bi utjecati cjelokupan sigurnosti oblaka. Upravljanje identitetom Azure AD povlaštene pomaže da biste riješili taj rizik smanjiti vrijeme za izlaganje ovlasti i povećanje vidljivosti u korištenje.  

Povlaštene upravljanje identitetom predstavlja pojam privremene administrator za ulogu ili "samo u vremenu" administratorski pristup, koja je korisnika koji treba da biste dovršili postupak sustava aktivacije za to dodijeljena uloga. Procesa aktivacije servisa mijenja Dodjela korisnika u ulogu u Azure AD iz Neaktivni Aktivno, u određenom vremenu razdoblje kao što su osam sati.

uči više:

- [Upravljanje identitetom povlaštene Azure AD](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Početak rada s Azure AD povlaštene upravljanje identitetom](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Zaštita identiteta

Zaštita identiteta za Azure Active Directory (AD) nudi konsolidirani prikaz sumnjive aktivnosti za prijavu i potencijalne slabe točke da biste zaštitili svoju tvrtku. Zaštita identiteta otkriva sumnjive aktivnosti koje korisnici i identiteta povlaštene (administratora), koji se temelji na signale kao što su napada grube sile leaked vjerodajnice i znak dodaci s nepoznatim mjesta i špijunskim uređaja.

Unosom obavijesti i preporučena olakšava zaštitu identiteta pomaže u smanjenju rizika u stvarnom vremenu. Izračunava težinu rizika korisnika, a možete konfigurirati utemeljen na rizika pravila automatski radi zaštite računala pristupiti iz budućih prijetnji.

uči više:

- [Zaštita identiteta Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanal 9: Azure AD i prikaz identiteta: Pretpregled zaštitu identiteta](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Centar za sigurnost

Centar za sigurnost Azure pomaže vam sprječavanje, otkrivanje i odgovaranje na prijetnji, a omogućuje povećati uvid u, i kontrolu nad, sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

Centar za sigurnost pomaže vam optimiziranje i nadzirati sigurnost Azure resurse tako da:

- Što vam omogućuje da definirate pravilnike za Azure pretplate resursa u skladu potrebama sigurnosti vaše tvrtke i vrstu aplikacije ili osjetljivosti podataka u svake pretplate.
- Praćenje stanja Azure virtualnim strojevima, mreže i aplikacije.
- Pruža popis prioritet sigurnosnih upozorenja, uključujući upozorenja s Integriranom partnerskih rješenja, uz informacije koje morate brzo Istraživanje i preporuke za remediate u slučaju napada.

uči više:

- [Uvod u Centar za sigurnost Azure](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
