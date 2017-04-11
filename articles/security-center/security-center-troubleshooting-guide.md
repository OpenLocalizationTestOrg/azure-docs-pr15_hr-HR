<properties
   pageTitle="Centar za sigurnost Azure vodiča za otklanjanje pogrešaka | Microsoft Azure"
   description="Otklanjanje poteškoća s centar za sigurnost Azure pomaže u ovaj dokument."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Centar za sigurnost Azure vodiča za otklanjanje poteškoća
Ovaj je vodič namijenjen za IT profesionalce (IT), informacije o sigurnosti analitičar analitičar podataka i administratori oblaka čije tvrtke ili ustanove koriste Azure centar za sigurnost i otklanjanje poteškoća s probleme vezane uz centar za sigurnost.

## <a name="troubleshooting-guide"></a>Vodič za otklanjanje poteškoća
Ovaj vodič u članku se objašnjava kako otkloniti probleme vezane uz centar za sigurnost. Većina otklanjanje poteškoća u Centar za sigurnost se odvija pregledom prvog zapisa [U zapisniku nadzora](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) za komponentu nije uspjelo. Pomoću zapisnika nadzora možete utvrditi:

- Operacijama okrenutim mjesto
- Tko je pokrenuo postupak
- Kada došlo je do postupka
- Status operacije
- Vrijednosti ostalih svojstava koji vam mogu pomoći Istraživanje postupka

Zapisnik nadzora sadrži sve operacije pisanja (broj, objavu, DELETE) izvršiti na resursa, no uvrstite operacije čitanja (DOHVATI).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Nadzor agent instalacije u sustavu Windows za otklanjanje poteškoća

Centar za sigurnost agent za nadzor koristi se za izvođenje prikupljanje podataka. Kada je omogućen prikupljanje podataka i agenta ispravno instalirana u ciljnom računalu, ti procesi mora biti u izvođenja:

- ASMAgentLauncher.exe - Azure nadzor Agent 
- ASMMonitoringAgent.exe - Azure nadzor sigurnost proširenja
- ASMSoftwareScanner.exe – Upravitelj Azure pregleda

Proširenje Azure sigurnost nadzor skenira za različite konfiguracija odgovarajuće sigurnosti i prikuplja sigurnost zapisnika s virtualnog računala. Pregled upravitelja bit će upotrijebljen kao zakrpu skener.

Ako se instalacija uspješno izvršiti trebali biste vidjeti slično onome ispod u zapisnika nadzora za ciljne VM unos:

![Zapisnika nadzora](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Dodatne informacije o postupak instalacije možete dobiti i tako da pročitate zapisnike agent koji se nalazi na *%systemdrive%\windowsazure\logs* (primjer: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Ako je misbehaving Agent centar za sigurnost Azure, morat ćete ponovno pokrenite cilj VM jer nema naredbe da biste prekinuli i pokretanje agenta.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Nadzor agent instalacija u Linux za otklanjanje poteškoća
Kada se instalacija VM Agent za otklanjanje poteškoća u sustavu Linux potrebno je provjeriti da biste/var/biblioteka/waagent/preuzet datotečni nastavak. Možete pokrenuti naredbu dolje da biste provjerili je li nije instaliran:

`cat /var/log/waagent.log` 

U druge datoteke zapisnika koje možete pregledati za otklanjanje poteškoća s svrhu su: 

- /VAR/log/mdsd.Err
- / var/zapisnika/azure /

U sustavu za rad na TCP 29130 trebali biste vidjeti vezu u tijeku mdsd. Ovo je syslog komunikaciju s mdsd postupak. Takvo ponašanje možete provjeriti tako da pokrenete naredbu u nastavku:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Obratite se Microsoftovoj službi za podršku

Neke od problema se identificirati pomoću upute navedene u ovom članku drugima možete pronaći i navedenih u javnom centar za sigurnost [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). No ako je potrebno Dodatno otklanjanje poteškoća, možete otvoriti novi zahtjev za podršku pomoću portala za Azure, kao što je prikazano u nastavku: 

![Microsoftova podrška](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako konfigurirati pravila zaštite u centru za sigurnost Azure. Da biste saznali više o centru za sigurnost Azure, pogledajte sljedeće:

- [Azure sigurnost centar za planiranje i vodič](security-center-planning-and-operations-guide.md) – upute za planiranje i razumijevanje zahtjevi dizajna prihvaćaju Azure centar za sigurnost.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resursi
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa
- [Azure sigurnost Blog](http://blogs.msdn.com/b/azuresecurity/) – pronaći bloga o Azure sigurnost i usklađenost
