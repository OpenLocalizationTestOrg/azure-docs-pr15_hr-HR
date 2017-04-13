<properties
   pageTitle="Infrastruktura crvena je vaša ažuriranja (RHUI) | Microsoft Azure"
   description="Saznajte više o crveno je vaša ažuriranja infrastrukture (RHUI) za instance crveno je vaša Enterprise Linux na zahtjev u Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Crvena je vaša ažuriranja infrastrukture (RHUI) za osvježavati crvena je vaša Enterprise Linux VMs servisu Azure

Da biste pristupili u crveno je vaša ažuriranja infrastrukture (RHUI) implementiran u Azure registrirane virtualnim strojevima stvorene iz osvježavati crveno je vaša Enterprise Linux (RHEL) slike dostupne u Azure Marketplace.  Instance RHEL osvježavati imaju pristup regionalne yum spremište i primati ažuriranja za rastuće.

Spremište popisu yum, koji upravlja RHUI, konfiguriran u vašoj instanci RHEL prilikom dodjele resursa. Ne morate učiniti dodatna konfiguracija – pokretanje `yum update` nakon vašoj instanci RHEL bude spremna za preuzeti najnovija ažuriranja.

> [AZURE.NOTE] Infrastruktura za Azure RHUI nedavno ažurirane (rujan 2016), a promjene u konfiguraciji pojavljivanja RHEL za zahtijeva osigurao pristup Azure RHUI. Pogledajte odjeljak ažuriranje infrastrukture za Azure RHUI detalje.


## <a name="rhui-azure-infrastructure-update"></a>Ažuriranje RHUI infrastruktura za Azure
Od 2016 rujan Azure ima novi skup crveno je vaša ažuriranja infrastrukture (RHUI) poslužitelja. Sljedećim poslužiteljima uvode se sa [Azure promet Upravitelj]( https://azure.microsoft.com/services/traffic-manager/) tako da jedan krajnju točku (rhui 1.micrsoft.com) može koristiti bilo koji VM bez obzira na to područje. Također koriste se SSL certifikata koji je povezane da biste poznati ustanova za izdavanje potvrda (Zagrebu korijenskog). Upućivanje ažuriranje automatsko bio opasan za neke korisnike koji imaju ACL-a ili prilagođene tablice usmjeravanja za ažuriranje poslužitelja RHUI tako da je ažuriranje "slaganja." Ručni koraci za za uhodavanje za tih novih poslužitelja dostupne su na ovu stranicu i dovršavanje skripta za za uhodavanje automatiziranog način (nakon provjere pojedinačne koraka). Novi RHEL PAYG slike u trgovine Windows Azure (verzije datumskim rujan 2016 ili noviji) će automatski pokažite na novih Azure RHUI poslužitelja, a ne zahtijeva sve dodatne akcije.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Novi Azure RHUI infrastrukture za uhodavanje vremenske trake

| Datum | Napomena |
| --- | --- |
|Rujan 22, 2016 | RHUI poslužitelji i upute o instalacija dostupan za korištenje. VMs uvesti pomoću nove (rujan 2016 datirana) RHEL PAYG trgovine slike automatski će koristiti novih poslužitelja RHUI, ali su postojeće VMs "slaganja"
|Studenom 1, 2016 | Naslijeđene RHEL PAYG VM slike (koji koriste stari poslužitelje Azure RHUI) uklonit će se iz galerije Azure Marketplace
|Siječanj 16, 2017 | Stari poslužitelje Azure RHUI će biti prestanka korištenja računala. Ažurirajte sve svoje problematične VMs PAYG RHEL tako da ovaj put da biste zadržali pristup Azure RHUI

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Se s IP-ovi za novih RHUI poslužitelja za isporuku sadržaja

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Ručno ažuriranje postupak da biste novih Azure RHUI poslužitelja

Preuzmite (putem zakretanja) javni ključ potpisa

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Provjerite je li preuzeti ključ

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Provjera Izlaz, provjerite je li `keyid` i `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Instalacija javni ključ

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Preuzimanje, provjerite je li i instalacija klijentskih RPM

Preuzimanje: Za RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Za RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Provjerite:

```
rpm -Kv azureclient.rpm
```

Provjera izlaza potpis paketa je u redu

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Instalacija sustava RPM

```
sudo rpm -U azureclient.rpm
```

Po dovršetku, provjerite mogu li pristupiti Azure RHUI obrasca na VM

### <a name="all-in-one-script-for-automating-the-above-task"></a>Sve-u-jednom skripte za automatiziranje iznad zadatka
Koristite sljedeću skriptu prema potrebi za automatizaciju zadataka ažuriranja problematične VMs novi Azure RHUI poslužiteljima.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Pregled RHUI
[Crvena je vaša ažuriranja infrastrukture](https://access.redhat.com/products/red-hat-update-infrastructure) nudi iznimno skalabilni rješenje za upravljanje sadržajem spremište yum crvena je vaša Enterprise Linux oblaka instanci koji se nalaze davatelji je vaša crveno certificirane oblaka. Na temelju upstream projekta Pulp RHUI omogućuje davatelja oblaka lokalno odražavati je vaša crveno hostira spremište sadržaja, stvoriti prilagođene spremištima s vlastite sadržajem i omogućiti te spremištima veliku skupinu krajnjim korisnicima putem sustava isporuke uravnoteženja sadržaja.

## <a name="regions-where-rhui-is-available"></a>Područja u kojem je dostupna RHUI
RHUI dostupna je u svim regijama koje su dostupne RHEL osvježavati slike. Trenutno uključuje sve javne regije navedene na stranicu [nadzorne ploče stanja Azure](https://azure.microsoft.com/status/) i Azure NAM državne područja. Pristup RHUI VMs dodjeli sa slike na zahtjev RHEL je sve obuhvaćeno njihova cijena. Dostupnost dodatne regionalne/otvorena oblaka ažurirat će se kao što smo proširite dostupnost RHEL na zahtjev u budućnosti.

> [AZURE.NOTE] Pristup Azure hostira RHUI ograničeno je na VMs unutar [Microsoft Azure podatkovnog centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Pribavljanje ažuriranja s drugom spremište ažuriranja

Ako vam je potrebna da biste primali ažuriranja iz spremišta različite ažuriranja (umjesto Azure hostira RHUI) morat ćete Poništavanje registriranja vaše instanci iz RHUI i ponovno registrirajte s željeni ažuriranje infrastrukture (primjerice crveno je vaša satelitski ili crveno je vaša kupca Portal CDN). Ćete odgovarajuće pretplate je vaša crvene boje za te servise i registracija [crveno](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)pristup putem oblaka razgovor u Azure.

Da biste unregister RHUI i registrirati svoje prati infrastrukture ažuriranje na ispod korake.

1.  Uredite /etc/yum.repos.d/rh-cloud.repo i promijenite sve `enabled=1` da biste `enabled=0`. Ako, na primjer:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  Uredite /etc/yum/pluginconf.d/rhnplugin.conf i promijenite `enabled=0` da biste `enabled=1`.
3.  Zatim registrirate željeni Infrastruktura, kao što su crveno je vaša kupca Portal. Slijedeći upute u vodiču za rješenja je vaša crveno kako [registrirati i pretplata sustava portalu kupca je vaša crvene boje](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Pristup RHUI Azure hostira je uključen u cijenu slika RHEL Pay-As-You-Go (PAYG). Poništavanje registriranja VM za RHEL PAYG iz RHUI Azure hostira ne pretvorite virtualnog računala u vrstu Premjesti-vaše – vlasnik – licence (BYOL) VM i zato vam možda biti povećavanja dvostruki naknade Ako registrirate isti VM s drugog izvora ažuriranja. 
> 
> Ako dosljedno trebate koristiti infrastruktura za ažuriranje osim Azure hostira RHUI razmislite o stvaranju i implementaciji vlastite slike (BYOL vrsta) kao što je opisano u članku [Stvaranje i prijenos je vaša crveno-poštu virtualnog računala za Azure](virtual-machines-linux-redhat-create-upload-vhd.md) .

## <a name="next-steps"></a>Daljnji koraci
Da biste stvorili Linux VM Enterprise je vaša crvene boje iz Azure Marketplace Pay-As-You-Go slike i leverage Azure hostira RHUI otvorite [Trgovine Windows Azure](https://azure.microsoft.com/marketplace/partners/redhat/). Moći koristiti `yum update` u instanci RHEL bez sve dodatne postavke.