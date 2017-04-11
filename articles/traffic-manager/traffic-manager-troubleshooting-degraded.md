<properties
    pageTitle="Otklanjanje poteškoća s smanjena status na Azure promet Manager"
    description="Otklanjanje poteškoća s profilima promet Upravitelj kada se prikazuje kao smanjena status."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Otklanjanje poteškoća s smanjena stanje na Azure promet Manager

U ovom se članku opisuje kako otkloniti poteškoće profil programa Azure promet Upravitelj prikazuju su smanjene status. U ovom scenariju uzmite u obzir da ste konfigurirali promet upravitelj profila koja pokazuje na neki od servisa cloudapp.net hostira. Kada je provjera stanja upravitelju promet, vidjet ćete da Status smanjena je funkcionalnost.

![su smanjene stanja](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Ako se na karticu krajnje točke taj profil, prikazuje jednu ili više krajnje točke s izvanmrežnog stanja:

![izvan mreže](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Razumijevanje promet Upravitelj probes

- Upravitelj promet smatra krajnje biti ONLINE samo kada se probni prima je odgovor HTTP 200 natrag iz probni put. -200 odgovor je pogreška.
- Preusmjeravanje za 30 x ne uspije, čak i ako preusmjereni URL vraća na 200.
- Za HTTPs probes zanemaruju se pogreške certifikata.
- Njegov sadržaj puta probni nije bitan pod uvjetom da se vraća na 200. Probing URL-a u nekim statički sadržaj, kao što su "/ favicon.ico" uobičajenih tehnika je. Dinamični sadržaj, kao što je stranica ASP možda neće uvijek vraćati 200, čak i kad je dobar aplikacije.
- Najbolja praksa je da biste postavili probni put na nešto što nije dovoljno logike utvrditi je li web-mjesto prema gore ili dolje. U prethodnom primjeru, postavljanjem put "/ favicon.ico", koje su samo testiranje tog w3wp.exe reagira. Ovaj probni može značiti da je web-aplikacije dobar. Bolje bio da biste postavili put na nešto kao što su "/ Probe.aspx" koja ima logike utvrđivanje stanja web-mjesta. Na primjer, možete koristiti mjerača performansi za procesora ili mjerenje broj zahtjeva nije uspjelo. Ili se pokušaju pristupiti baze podataka resursa ili stanje sesije da biste provjerili funkcionira li web-aplikaciju.
- Ako su sve krajnje točke u profilu smanjena, zatim Upravitelj promet tretira sve krajnje točke kao dobar i usmjerava promet za sve krajnje točke. Takvo ponašanje osigurava da probleme s probing mehanizam neće rezultirati dovršeno nedostupnosti servisa.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Da biste otklonili pogreške probni, potrebno vam je alat koji prikazuje Šifra stanja HTTP povrata iz probni URL-a. Postoji mnogo alatima koji vam pokazuju sirovim HTTP odgovor.

* [Fiddler](http://www.telerik.com/fiddler)
* [zakretanja](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Osim toga, možete koristiti kartice mreža alata za ispravljanje pogrešaka F12 u pregledniku Internet Explorer da biste pogledali HTTP odgovore.

U ovom primjeru želimo da biste vidjeli odgovor probni URL: http://watestsdp2008r2.cloudapp.net:80/probni. U sljedećem primjeru PowerShell ilustrira problem.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Primjer izlaz:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Obratite pozornost na to ćemo primio odgovor preusmjeravanje. Prema uputama koje ste prethodno, sve kôd statusa drugim od 200 smatra se pogreška. Upravitelj promet mijenja status krajnje točke na izvanmrežno. Da biste riješili taj problem, provjerite konfiguracije web-mjesta da biste bili sigurni da proper kôd statusa može vratiti probni put. Konfigurirajte probni Upravitelj promet na put koji se prikazuje na 200.

Ako vaš probni koristi HTTPS protokola, možda ćete morati onemogućite provjeru za izbjegavanje pogrešaka SSL/TLS tijekom testiranju certifikata. Sljedeće naredbe ljuske PowerShell onemogućivanje provjeru valjanosti certifikata za trenutnu sesiju ljuske PowerShell:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Daljnji koraci

[O upravitelju promet promet usmjeravanje metode](traffic-manager-routing-methods.md)

[Što je upravitelj promet](traffic-manager-overview.md)

[Servisi u oblaku](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure web-aplikacije](https://azure.microsoft.com/documentation/services/app-service/web/)

[Operacije na Upravitelj promet (REST API referenca)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure promet Upravitelj cmdleta][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
