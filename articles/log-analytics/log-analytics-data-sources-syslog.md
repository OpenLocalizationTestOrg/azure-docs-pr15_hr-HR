<properties 
   pageTitle="Syslog poruke u prijava analitiku | Microsoft Azure"
   description="Syslog je protokol zapisivanje za događaj koji se nalazi zajednička Linux.   U ovom se članku objašnjava kako konfigurirati skup Syslog poruka u zapisnik analize i pojedinosti zapisa koji se stvaraju u spremištu OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Izvori podataka Syslog u zapisnik Analytics

Syslog je protokol zapisivanje za događaj koji se nalazi zajednička Linux.  Aplikacija će poslati poruke koje možda pohranjena na lokalnom računalu ili isporučena Syslog prikupljanje.  Kada instalirate OMS Agent za Linux konfigurira lokalni Syslog daemon proslijediti poruke agenta.  Agenta zatim šalje poruku zapisnika analize u spremištu OMS na kojem je stvorena odgovarajući zapis.  

> [AZURE.NOTE]Prijava analitiku podržava skup poruke poslane rsyslog ili syslog pokazuju. Zadani daemona syslog verzije 5 crveno je vaša Enterprise Linux, CentOS i Oracle Linux verzije (sysklog) nije podržana za zbirke syslog događaja. Da biste prikupili podatke syslog iz ove verzije ovih distribucija, [rsyslog daemon](http://rsyslog.com) mora biti instalacije i konfiguracije da biste zamijenili sysklog.

![Zbirka Syslog](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Konfiguriranje Syslog
Događaji s objektima i severities koje su navedene u svoju konfiguraciju će prikupljati samo OMS Agent za Linux.  Syslog možete konfigurirati putem portala za OMS ili putem upravljanja konfiguracijske datoteke na vašem agenata Linux.


### <a name="configure-syslog-in-the-oms-portal"></a>Konfiguriranje Syslog na portalu OMS

Konfiguriranje Syslog na [izborniku Podaci u postavke zapisnika analize](log-analytics-data-sources.md#configuring-data-sources).  Tu konfiguraciju je isporučena u konfiguracijskoj datoteci na svaki pojedini agent Linux.

Možete dodati nove funkcijom tako da pritisnete u nazivu, a zatim kliknete **+**.  Za svaki funkcijom samo poruke s odabranog severities prikupljaju se.  Provjerite severities za određenu funkciju koja želite prikupiti.  Ne možete unijeti kriterije za filtriranje poruka.

![Konfiguriranje Syslog](media/log-analytics-data-sources-syslog/configure.png)


Prema zadanim postavkama, sve promjene konfiguracije automatski pomiču se za sve agenata.  Ako želite ručno konfiguriranje Syslog na svaki pojedini agent Linux, poništite okvir *Primijeni ispod konfiguracije Moje strojeva Linux*.


### <a name="configure-syslog-on-linux-agent"></a>Konfiguriranje Syslog na Linux agent

Kada [na Linux klijenta koji je instaliran OMS agent](log-analytics-linux-agents.md), instalira konfiguracijska datoteka syslog zadani koji definira funkcijom i težinu poruke koje su koji se prikupljaju.  Možete izmijeniti ove datoteke da biste promijenili konfiguraciju.  Konfiguracijska datoteka razlikuje se ovisno o daemon Syslog koji je instaliran klijent.

> [AZURE.NOTE] Ako uređujete konfiguracije syslog, morate ponovno pokrenuti daemon syslog da bi promjene stupile na snagu.

#### <a name="rsyslog"></a>rsyslog

Konfiguracijska datoteka za rsyslog nalazi se na **/etc/rsyslog.d/95-omsagent.conf**.  Zadani sadržaj možete prikazano u nastavku.  To prikuplja syslog poruka iz lokalne agent za sve objekte s razinom upozorenja ili noviji.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Funkciju možete ukloniti tako da uklonite njezin dio konfiguracijskoj datoteci.  Možete ograničiti severities koje su prikupiti za određeni funkcijom izmjenom te funkcijom unosa.  Ako, na primjer, da biste ograničili funkciju korisnika na poruke s težinu pogreške ili noviji želite izmijeniti tom retku konfiguracijska datoteka za sljedeće:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>syslog pokazuju

Konfiguracijska datoteka za rsyslog je **/etc/syslog-ng/syslog-ng.conf**na mjestu.  Zadani sadržaj možete prikazano u nastavku.  To prikuplja syslog poruka iz lokalne agent za sve objekte i sve severities.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Funkciju možete ukloniti tako da uklonite njezin dio konfiguracijskoj datoteci.  Možete ograničiti severities koji se prikupljaju za određeni funkcijom tako da ih uklonite svoj popis.  Na primjer, da biste ograničili funkciju korisnika na samo upozorenja i ključnih poruke, želite izmijeniti dio konfiguracijska datoteka za sljedeće:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Promjena Syslog priključak

Agent za OMS prati Syslog poruka na lokalno klijentsko pri priključak 25224.  Ovaj priključak možete promijeniti tako da dodate u sljedećoj sekciji nalazi se na **/etc/opt/microsoft/omsagent/conf/omsagent.conf**OMS agent konfiguracijskoj datoteci.  Broj priključka koji želite zamijeniti 25224 u stavci **priključak** .  Imajte na umu da i morat ćete izmjena konfiguracijska datoteka za daemon Syslog za slanje poruka s tog priključka.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Prikupljanje podataka

Agent za OMS prati Syslog poruka na lokalno klijentsko pri priključak 25224. Konfiguracijska datoteka za Syslog daemon prosljeđuje Syslog poruke poslane iz aplikacije priključak gdje su prikuplja zapisnika analize.


## <a name="syslog-record-properties"></a>Syslog svojstvima zapisa

Zapisi Syslog vrstu **Syslog** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Računalo | Računalo na kojem je prikupljenih događaj. |
| Funkcijom | Definira dio sustava koji je generirao poruku. |
| HostIP | IP adrese sustava slanja poruke.  |
| Naziv glavnog računala | Naziv sustava slanja poruke. |
| SeverityLevel | Razina težinu događaj. |
| SyslogMessage | Tekst poruke. |
| ProcessID | ID procesa koji je generirao poruku. |
| EventTime | Datum i vrijeme koja je stvorena događaj.



## <a name="log-queries-with-syslog-records"></a>Upiti zapisnik sa zapisima Syslog

Sljedeća tablica sadrži različite Primjeri zapisnika upita koji dohvaćaju Syslog zapisa.

| Upit | Opis |
|:--|:--|
| Vrsta = Syslog | Sve Syslogs. |
| Vrsta = Syslog SeverityLevel = pogreške | Svi zapisi Syslog s težinu pogreške. |
| Vrsta = Syslog & #124; Izmjerite count() na računalu | Broj Syslog zapisa na računalu. |
| Vrsta = Syslog & #124; Izmjerite count() prema funkciji | Broj Syslog zapisa po funkcijom. |

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja. 
- Pomoću [Prilagođenih polja](log-analytics-custom-fields.md) analizirati podatke iz zapisa syslog u pojedinačna polja.
- [Konfiguriranje Linux agenata](log-analytics-linux-agents.md) da biste prikupili druge vrste podataka. 
