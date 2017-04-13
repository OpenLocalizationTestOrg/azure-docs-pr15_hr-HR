<properties 
   pageTitle="Prikaz i izmjena Hostnames | Microsoft Azure"
   description="Upute za prikaz i promjena hostnames za Azure virtualnih računala, web- i tempiranja ulogama za razlučivanje naziva"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Prikaz i izmjena hostnames

Da biste omogućili vaša uloga instanci upućivati na glavno računalo, morate postaviti vrijednost za naziv glavnog računala u konfiguracijskoj datoteci servisa za svaku ulogu. To učiniti tako da dodate na željeni naziv glavnog računala atribut **vmName** elementa **uloge** . Vrijednost atributa **vmName** služi kao osnovu za naziv glavnog računala svaku instancu uloge. Na primjer, ako **vmName** je *webrole* i postoje tri instance te uloge, nazivi glavnog računala na instanci bit će *webrole0*, *webrole1*i *webrole2*. Nije potrebno navesti naziv glavnog računala za virtualnim strojevima u konfiguracijskoj datoteci jer je naziv glavnog računala za virtualnog računala popunjeno temelji se na nazivu virtualnog računala. Dodatne informacije o konfiguriranju servisa Microsoft Azure potražite u članku [Azure servisa konfiguracije shema (.cscfg datoteke)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Prikaz hostnames

Nazivi glavnog računala virtualnim strojevima i uloga instance možete pogledati u oblaku putem alata u nastavku.

### <a name="azure-portal"></a>Portal za Azure

[Portal za Azure](http://portal.azure.com) možete koristiti da biste vidjeli nazivi glavnog računala za virtualnim strojevima na pregled plohu za virtualnog računala. Imajte na umu da se plohu prikazuje vrijednost za **ime** i **Naziv glavnog računala**. Iako su prethodno isti, mijenjati naziv glavnog računala neće se promijeniti naziv virtualnog računala ili uloga instance.

Uloga instance moguće je prikazati i na portalu za Azure, ali kada popis instanci u oblaku, ne prikazuje naziv glavnog računala. Prikazat će se naziv za svaku instancu, no taj naziv predstavlja naziv glavnog računala.

### <a name="service-configuration-file"></a>Konfiguracijska datoteka servisa

Konfiguracijska datoteka servisa distribuiranih servisa možete preuzeti iz plohu **Konfiguriranje** servisa na portalu za Azure. Zatim možete potražiti atribut **vmName** za element **Naziv uloge** da biste vidjeli naziv glavnog računala. Imajte na umu da taj naziv glavnog računala koristiti kao osnovu za naziv glavnog računala svaku instancu uloge. Na primjer, ako **vmName** je *webrole* i postoje tri instance te uloge, nazivi glavnog računala na instanci bit će *webrole0*, *webrole1*i *webrole2*.

### <a name="remote-desktop"></a>Udaljena radna površina

Kada omogućite udaljene radne površine (Windows), komponente Windows PowerShell remoting (Windows) ili SSH (Linux i Windows) veze na virtualnim strojevima ili uloga instance možete prikazati naziv glavnog računala na aktivnu vezu udaljene radne površine na različite načine:

- U naredbeni redak ili SSH terminal upišite naziv glavnog računala.

- Upišite ipconfig/sve na naredbu Pitaj (samo za Windows).

- Prikaz naziva računala u postavkama sustava (samo za Windows).

### <a name="azure-service-management-rest-api"></a>Upravljanje servisom za Azure REST API-JA

Od ostalih klijenta, slijedite ove upute:

1. Provjerite je li potvrdu klijenta za povezivanje s portala za Azure. Da biste dobili potvrdu klijenta, slijedite korake navedene u [Kako: preuzimanje i objavljivanje postavki uvoza i informacije o pretplati](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Postavite stavku zaglavlje s nazivom x-ms-verzija s vrijednošću 2013, 11 i 01.

1. Slanje zahtjeva za u sljedećem obliku: https://management.core.windows.net/\<subscrition id\>/services/hostedservices/\<naziv usluge\>?embed detalja = true

1. Obratite pažnju na element **naziv glavnog računala** za svaki element **RoleInstance** .

>[AZURE.WARNING] Nastavak interne domene možete pogledati i servisa oblaka iz poziva odgovor OSTALE potvrđivanjem **InternalDnsSuffix** element ili tako da pokrenete ipconfig/sve iz naredbenog retka u sesiji udaljene radne površine (Windows) ili izvodi mačka /etc/resolv.conf iz SSH terminal (Linux).

## <a name="modifying-a-hostname"></a>Izmjena naziv glavnog računala

Naziv glavnog računala za svaku virtualnog računala ili uloga instance možete promijeniti tako da prenesete izmijenjene servisa konfiguracijska datoteka ili promjenom naziva računala iz sesiji udaljene radne površine.

## <a name="next-steps"></a>Daljnji koraci

[Razrješavanje imena u pošti (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Shema konfiguracije Azure Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Shema konfiguracije Azure virtualne mreže](http://go.microsoft.com/fwlink/?LinkId=248093)

[Određivanje postavki DNS-a pomoću mrežnih konfiguracije datoteka](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
