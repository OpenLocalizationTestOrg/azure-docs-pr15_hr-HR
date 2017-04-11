<properties
   pageTitle="IIS zapisnike u prijava analitiku | Microsoft Azure"
   description="Internet Information Services (IIS) sprema aktivnosti korisnika u datoteke zapisnika koje je moguće prikupiti zapisnika analize.  U ovom se članku objašnjava kako konfigurirati prikupljanje zapisnika IIS i pojedinosti zapisa koji se stvaraju u spremištu OMS."
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
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="iis-logs-in-log-analytics"></a>IIS zapisnike u zapisnik Analytics
Internet Information Services (IIS) sprema aktivnosti korisnika u datoteke zapisnika koje je moguće prikupiti zapisnika analize.  

![IIS zapisnika](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfiguriranje programa IIS prijavi
Prijava analitiku prikuplja unose iz datoteka zapisnika stvorenih zapisivanjem IIS, tako da najprije morate [konfigurirati IIS za zapisivanje](https://technet.microsoft.com/library/hh831775.aspx).

Prijava analitiku samo podržava IIS zapisničke datoteke spremljene u obliku W3C i ne podržava prilagođena polja ili IIS napredne bilježenje u zapisnik.  
Prijava analitiku prikupljanje zapisnika u izvornom obliku NCSA ili IIS.

Konfiguriranje zapisnika IIS u zapisnik analize [podataka izbornik u postavke zapisnika analize](log-analytics-data-sources.md#configuring-data-sources).  Postoji bez konfiguracije obavezno osim tako da odaberete **Datoteka zapisnika prikupljanje W3C oblik IIS**.

Preporučujemo da kada omogućite prikupljanje zapisnika IIS na svakom poslužitelju potrebno je konfigurirati prilikom prelaska postavku za IIS zapisnika.


## <a name="data-collection"></a>Prikupljanje podataka

Prijava analitiku prikuplja unose u zapisnik IIS svakog izvora povezanih otprilike svakih 15 minuta.  Agenta bilježi umjesto nje svaki događaj prikuplja iz.  Ako agenta vodi izvan mreže, zatim prijava analitiku prikuplja događaje s mjesta na kojem je zadnji put stali, čak i ako događaje stvorene dok agenta je izvan mreže.


## <a name="iis-log-record-properties"></a>Svojstva zapisa za IIS zapisnika

IIS zapisnika zapisa vrstu **W3CIISLog** i imate svojstava u tablici u nastavku:

| Svojstvo | Opis |
|:--|:--|
| Računalo | Naziv računala na kojemu je prikupljenih događaj. |
| cIP | IP adresa klijenta. |
| csMethod | Način zahtjev kao što su GET ili objavu. |
| csReferer | Web-mjesto je korisnik nakon čega vezu s trenutno web-mjesto. |
| csUserAgent | Vrste klijenta u pregledniku. |
| csUserName | Ime korisnika čija je autentičnost provjerena kojima pristupiti poslužitelju. Anonimni korisnici su označenu spojnice. |
| csUriStem | Cilj zahtjev kao što je web-stranicu. |
| csUriQuery | Upit, ako postoje, da biste izvršili pokušao klijent. |
| ManagementGroupName | Naziv grupe za upravljanje Operations Manager agenata.  Za ostale agenata to je AOI -\<ID radnog prostora\> |
| RemoteIPCountry | Država IP adresa klijenta. |
| RemoteIPLatitude | Zemljopisnu širinu IP adresu klijenta. |
| RemoteIPLongitude | Dužina IP adresu klijenta. |
| scStatus | Šifra stanja HTTP-a. |
| scSubStatus | Kod pogreške substatus. |
| scWin32Status | Šifra stanja sustava Windows. |
| sIP | IP adresa web-poslužitelj. |
| SourceSystem  | OpsMgr |
| sPort | Povezani priključaka na poslužitelju klijent. |
| sSiteName | Naziv IIS web-mjesta. |
| TimeGenerated | Datum i vrijeme je bio prijavljen unosa. |
| TimeTaken | Razdoblje obrade zahtjeva u milisekundama. |

## <a name="log-searches-with-iis-logs"></a>Zapisnika pretraživanja s IIS zapisnika

Sljedeća tablica sadrži različite Primjeri zapisnika upite koji se dohvaćanje zapisa IIS zapisnika.

| Upit | Opis |
|:--|:--|
| Vrsta = IISLog | Svi zapisi IIS zapisnika. |
| Vrsta = IISLog EventLevelName = pogreške | Svi događaji Windows s težinu pogreške. |
| Vrsta = W3CIISLog & #124; Izmjerite count() po cIP | Count IIS stavke zapisnika po IP adresu klijenta. |
| Vrsta = W3CIISLog csHost = "www.contoso.com" & #124; Izmjerite count() po csUriStem | Count IIS prijavite stavke po URL www.contoso.com glavnog računala. |
| Vrsta = W3CIISLog & #124; Izmjerite Sum(csBytes) tako da računalni i #124; Vrh 500000| Ukupno bajtova primio svako računalo IIS. |

## <a name="next-steps"></a>Daljnji koraci

- Konfiguriranje prijava analitiku u prikupljanje drugih [izvora podataka](log-analytics-data-sources.md) za analizu.
- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja.
- Konfiguriranje upozorenja u zapisnik analize doći obavijesti o važne uvjete koji se nalaze u IIS zapisnika.
