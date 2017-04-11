<properties
    pageTitle="Pristup ključ sigurnog iza vatrozida | Microsoft Azure"
    description="Saznajte kako pristupiti sigurnog ključ iz aplikacije iza vatrozida"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Pristup ključ sigurnog iza vatrozida
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>P: ključa sigurnog klijentska aplikacija mora biti iza vatrozida, koje priključke/domaćini/IP adrese trebali biste otvoriti da biste omogućili pristup ključa sigurnog?

Da biste pristupili ključa sigurnog klijentsku aplikaciju sigurnog ključa mora moći pristupiti više end-points za različite radovi.

- Provjera autentičnosti (putem Azure Active Directory)
- Upravljanje ključ sigurnog (koji obuhvaćaju stvaranje/pročitano/ažuriranje i brisanje te postavke pravila za pristup) putem upravitelja za Azure resursa
- Pristup i upravljanje objekte (tipke i tajne) pohranjene u ključa sigurnog sam, prolaze kroz ključa sigurnog određene krajnju točku (npr. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Ovisno o konfiguraciji i okruženja, postoje neka varijacije.   

## <a name="ports"></a>Priključci

Sav promet ključa zbirke ključeva za sve 3 funkcije (Provjera autentičnosti, upravljanje i podataka ravnini pristup) prelazi preko HTTPS: priključak 443. No za CRL-ova, pojavit će se povremeno promet HTTP (priključak 80). Klijenti koji podržavaju OCSP bi trebalo dosegne CRL-ova, ali ponekad može pristupiti [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Provjera autentičnosti

Ključ sigurnog klijentska aplikacija ćete pristupati krajnje točke Azure Active Directory za provjeru autentičnosti. Krajnja točka koristi ovisi o klijentu konfiguracije AAD i vrstu glavnice – Glavno ime korisnika, usluge i vrstu računa, primjerice Microsoftov Account ili ID tvrtke ili ustanove.  

| Vrsta upravitelja | Krajnja točka: priključak |
|----------------|---------------|
| Korisnika pomoću Microsoftova Account<br> (npr.user@hotmail.com) | **Globalni:**<br> login.microsoftonline.com:443<br><br> **Azure Kina:**<br> login.chinacloudapi.CN:443<br><br>**Azure vlade sad:**<br> Prijava us.microsoftonline.com:443<br><br>**Azure Njemačka:**<br> login.microsoftonline.de:443<br><br> i <br>login.Live.com:443   |
| Korisnički servis glavnicu pomoću pomoću ID-a AAD (npr.user@contoso.com) | **Globalni:**<br> login.microsoftonline.com:443<br><br> **Azure Kina:**<br> login.chinacloudapi.CN:443<br><br>**Azure vlade sad:**<br> Prijava us.microsoftonline.com:443<br><br>**Azure Njemačka:**<br> login.microsoftonline.de:443 |
| Korisnički servis glavnicu pomoću organizacijske ID + ADFS ili druge krajnje vanjsko (npr.user@contoso.com) | Sve iznad krajnjih točaka za pomoću ID-a plus ADFS ili nekog drugog vanjskog krajnje točke |

Postoje drugi moguće složene scenariji. Pogledajte [Azure Active Directory provjere autentičnosti tijeka](/documentation/articles/active-directory-authentication-scenarios/), [Integracija aplikacija pomoću servisa Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) i [Active Directory provjere autentičnosti protokoli](https://msdn.microsoft.com/library/azure/dn151124.aspx) za dodatne informacije.  

## <a name="key-vault-management"></a>Upravljanje ključem zbirke ključeva

Za sigurnog upravljanje ključevima (CRUD i postavljanje pravilnik o pristupu), klijentska aplikacija ključa sigurnog treba pristupiti Voditelj resursa Azure krajnjoj točki.  

| Vrste operacija | Krajnja točka: priključak |
|----------------|---------------|
| Ključ sigurnog kontrola ravnini operacije<br> putem Azure Voditelj resursa | **Globalni:**<br> Management.Azure.com:443<br><br> **Azure Kina:**<br> Management.chinacloudapi.CN:443<br><br> **Azure vlade sad:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Njemačka:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory grafikonu API-JA | **Globalni:**<br> Graph.Windows.NET:443<br><br> **Azure Kina:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure vlade sad:**<br> Graph.Windows.NET:443<br><br> **Azure Njemačka:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Ključni sigurnog operacije

Za sve ključne sigurnog objekta (tipke i tajne) upravljanja i šifriranja operacije ključa sigurnog klijenta mora da biste pristupili ključa sigurnog krajnju točku. Ovisno o lokaciji vaše sigurnog ključ razlikuje se DNS sufiks krajnjoj točki. Završna točka sigurnog ključ je u obliku: < sigurnog naziv >. < regija-određene-dns-sufiks > kao što je opisano u tablici u nastavku.  

| Vrste operacija | Krajnja točka: priključak |
|----------------|---------------|
| Ključ sigurnog operacija kao što su šifrirane operacije tipki, tipke stvoreno/pročitano/ažuriranje i brisanje i tajne, skup/get oznake i druge atribute na ključa sigurnog objektima (tipke na tajne)     | **Globalni:**<br> &lt;sigurnog naziv&gt;. vault.azure.net:443<br><br> **Azure Kina:**<br> &lt;sigurnog naziv&gt;. vault.azure.cn:443<br><br> **Azure vlade sad:**<br> &lt;sigurnog naziv&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Njemačka:**<br> &lt;sigurnog naziv&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>Rasponi IP adresa

Servisa sigurnog ključ shodno koristi druge Azure resurse kao što su PaaS infrastrukture, dakle nije moguće omogućuje određeni raspon IP adresama krajnje točke servisa sigurnog ključa će se nalaziti u bilo kojem trenutku. No ako vatrozida podržava samo IP adresa raspona pa potražite dokument [Microsoft Azure podatkovnog centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653) .   Za provjeru autentičnosti i identiteta (Azure Active Directory), aplikacija mora biti možete povezati s krajnje točke opisane u [provjeru autentičnosti i identitet adrese](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Daljnji koraci

- Ako imate pitanja o sigurnog ključ, posjetite [Forume sigurnog ključ za Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
