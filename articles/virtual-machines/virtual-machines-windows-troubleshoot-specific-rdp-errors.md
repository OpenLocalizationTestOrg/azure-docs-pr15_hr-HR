<properties
    pageTitle="Određeni RDP poruke o pogreškama za Azure VMs | Microsoft Azure"
    description="Razumijevanje određene poruke o pogreškama koje se mogu pojaviti prilikom pomoću veze udaljene radne površine da biste virtualnog računala za Windows na Azure"
    keywords="Udaljene radne površine pogrešku, pogreška udaljene radne površine, ne može povezati s VM udaljene radne površine otklanjanje poteškoća"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Otklanjanje poteškoća s određenim RDP poruke o pogreškama u sustavu Windows VM servisu Azure
Možete dobiti na određenu poruku o pogrešci prilikom korištenja udaljene radne površine veza Windows virtualnog računala (VM) u Azure. U ovom se članku detaljno neke od najčešćih poruka o pogrešci naiđe na, zajedno s korake da biste ih riješili za otklanjanje poteškoća. Ako imate poteškoća povezati svoje VM pomoću RDP, ali naići na određenu poruku o pogrešci, potražite u članku [Vodič za udaljene radne površine za otklanjanje poteškoća](virtual-machines-windows-troubleshoot-rdp-connection.md).

Informacije o određena poruka o pogrešci, potražite u sljedećim člancima:

- [Udaljena sesija prekinuta jer nema udaljene radne površine licence poslužiteljima koji daju te licence](#rdplicense).
- [Udaljena radna površina ne možete pronaći računalo "naziv"](#rdpname).
- Došlo je do pogreške [za provjeru autentičnosti. Ne možete uspostaviti lokalnoj ustanovi za sigurnost](#rdpauth).
- [Sigurnost sustava Windows pogreška: nije uspjelo vjerodajnice](#wincred).
- [Ovo računalo ne može povezati s udaljenog računala](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Udaljena sesija prekinuta je jer nema udaljene radne površine licence poslužiteljima koji daju te licence.

Uzrok: Istekla 120 dana licenciranja razdoblja mirovanja za uloga poslužitelja udaljene radne površine i morate instalirati licence.

Kao zaobilazno rješenje, spremiti lokalnu kopiju datoteke RDP s portala sustava i pokrenite sljedeću naredbu u naredbenom retku PowerShell da biste se povezali. Ovaj korak onemogućuje licenciranje za samo tu vezu:

        mstsc <File name>.RDP /admin

Ako zapravo ne trebate više od dva istodobno udaljene radne površine veze na VM, Upravitelj poslužitelja možete koristiti da biste uklonili uloga poslužitelja udaljene radne površine.

Dodatne informacije potražite u članku na blogu [Azure VM ne uspijeva s "Nema udaljene radne površine licence poslužiteljima"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Udaljena radna površina ne može pronaći računalo "naziv".

Uzrok: Klijent udaljene radne površine na računalu ne može odrediti naziv računala u postavkama RDP datoteci.

Moguća rješenja:

- Ako ste na intranetu tvrtki ili ustanovi, provjerite je li vaše računalo ima pristup proxy poslužitelj, a možete poslati HTTPS promet na njega.

- Ako koristite lokalne RDP datoteci, pokušajte koristiti onaj koji je generirao portalu. Na taj način imate li točan naziv DNS-a za virtualnog računala ili servisa u oblaku i priključak krajnje točke na VM. Evo ogledne datoteke RDP generira portalu:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Dijelu RDP datoteka sadrži:
- Na potpuno kvalificirani naziv domene od servisa u oblaku koja sadrži VM ("tailspin-azdatatier.cloudapp.net" u ovom primjeru).

- Vanjski TCP priključak krajnje točke za promet udaljene radne površine (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Došlo je do pogreške provjere autentičnosti. Ne možete uspostaviti lokalnoj ustanovi za sigurnost.

Uzrok: Cilj VM ne može pronaći ustanovi za sigurnost u dijelu korisničko ime vjerodajnice.

Kada je korisničko ime u obliku *SecurityAuthority*\\*korisničko ime* (primjer: CORP\User1), *SecurityAuthority* dio je naziv računala na VM (za izdavanje lokalne sigurnosne) ili je naziv domene servisa Active Directory.

Moguća rješenja:

- Ako je račun za na VM lokalni, provjerite je li naziv VM pravilno napisane.

- Ako je račun na domeni Active Directory, provjerite naziv domene.

- Ako je račun servisa Active Directory domene, a naziv domene nije ispravno upisan, provjerite je li kontroler domene dostupan u toj domeni. Je uobičajenih problema u Azure virtualne mreže koje sadrže kontrolera domena kontroler domene nije dostupna jer nije pokrenut. Kao zaobilazno rješenje, umjesto račun domene možete koristiti lokalni administratorski račun.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Sigurnost sustava Windows pogreška: nije uspjelo vjerodajnice.

Uzrok: Cilj VM ne može provjeriti vaše ime za račun i lozinku.

Računalo sa sustavom Windows možete provjeriti vjerodajnica za lokalni račun ili račun domene.

- Da biste postigli lokalni računi koristite *ComputerName*\\sintaksa*korisničko ime* (primjer: SQL1\Admin4798).
- Domenski računi pomoću *DomainName*\\sintaksa*korisničko ime* (primjer: CONTOSO\peterodman).

Ako imate ulogu sustava VM s kontrolerom domene u novi skup stabala Active Directory, lokalni administratorski račun koji ste se prijavili pomoću se pretvara u ekvivalentne računa koji ima istu lozinku u novi skup stabala i domenu. Zatim se briše lokalni račun.

Ako, na primjer, ako prijavljeni lokalan DC1\DCAdmin, a zatim proslijediti virtualnog računala kao kontroler domene u skup stabala za novo corp.contoso.com domene, briše DC1\DCAdmin lokalni račun i stvaranja novog računa domene (CORP\DCAdmin) s istu lozinku.

Provjerite je li naziv računa nije je naziv koji virtualnog računala možete provjeriti kao valjani račun i lozinka točni.

Ako je potrebno promijeniti lozinku lokalnog administratorskog računa potražite u članku [upute za ponovno postavljanje lozinke ili servisu udaljene radne površine za virtualnim strojevima sa sustavom Windows](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Ovo računalo ne može povezati s udaljenog računala.

Uzrok: Račun koji se koristi za povezivanje nema udaljene radne površine prijavu prava.

Svaki računalo sa sustavom Windows sadrži grupu Lokalni korisnici udaljene radne površine, koja sadrži račune i grupe koji mogu se prijaviti u nju daljinski. Članovi lokalne grupe administratora i imaju pristup, čak i ako te račune nisu navedeni u lokalne grupe korisnika udaljene radne površine. Za domenu pridruženo strojeva lokalne grupe administratora sadrži administratori domene za domenu.

Provjerite ima li račun koji koristite da biste se povezali s udaljene radne površine prijavu prava. Kao zaobilazno rješenje, koristite domene ili lokalnog administratorskog računa da biste se povezali putem udaljene radne površine. Da biste dodali željeni račun lokalne grupe korisnika udaljene radne površine, koristite dodatak za Microsoft Management Console (**Alati sustava > lokalne korisnike i grupe > grupe > korisnici udaljene radne površine**).


## <a name="next-steps"></a>Daljnji koraci
Ako nijedan od tih pogrešaka došlo je do, a imate nepoznate problema s povezivanjem pomoću RDP, potražite u članku [Vodič za udaljene radne površine za otklanjanje poteškoća](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Paket za dijagnostiku Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Upute za otklanjanje poteškoća u pristupa aplikacije koje rade na na VM, potražite u članku [Otklanjanje poteškoća s pristupom aplikacije koje se izvode na programa Azure VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Ako imate poteškoća pomoću sigurne ljuske (SSH) za povezivanje s Linux VM servisu Azure, potražite u članku [Otklanjanje poteškoća s SSH veze Linux VM u Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).