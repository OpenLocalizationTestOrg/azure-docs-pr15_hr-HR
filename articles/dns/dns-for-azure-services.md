<properties
  pageTitle="Korištenje Azure DNS s drugih servisa za Azure | Microsoft Azure"
  description="Objašnjenje kako koristiti Azure DNS-a da biste riješili naziv drugih servisa za Azure"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Korištenje Azure DNS s drugih servisa za Azure

Azure DNS je DNS upravljanje i naziv razlučivost servis. Omogućuje stvaranje javne DNS naziva za druge aplikacije i servise koje ste implementiran u Azure. Stvaranja naziva za Azure service u vašu prilagođenu domenu jednostavan je dodavanja zapisa ispravne vrste servisa.

* Za dinamično dodijeljene IP adrese, morate stvoriti DNS CNAME zapis koji se preslikava DNS naziv stvorenog Azure servisa. DNS standarde onemogućite korištenje CNAME zapis za vrh zone.
* Za statički dodijeljene IP adrese, možete stvoriti pomoću bilo koje ime, uključujući naziv _naked domene_ na vrh zone DNS-A zapis.

Sljedeća tablica prikazuje podržane vrste podataka koji se može koristiti za različite servise za Azure. Kao što možete vidjeti iz ove tablice, Azure DNS podržava samo DNS zapise za mjesto na Internetu mrežni resursi. Azure DNS nije moguće koristiti za razlučivanje naziva Interna, Privatna adresa.

| Servis za Azure | Mrežno sučelje | Opis |
|---------------|-------------------|-------------|
| Pristupnik za aplikaciju | Pristupne javnu IP | Možete stvoriti DNS-A ili CNAME zapis. |
| Opterećenja | Pristupne javnu IP | Možete stvoriti DNS-A ili CNAME zapis. Opterećenja može imati IPv6 javnu IP adresu koja se dinamički dodijeljeni. Dakle, morate stvoriti CNAME zapis za IPv6 adrese. |
| Upravitelj promet | Javni naziv | Možete stvoriti samo CNAME mapira trafficmanager.net naziv dodijeljen promet upravitelj profila. Dodatne informacije potražite u članku [kako promet Upravitelj funkcionira](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Servis u oblaku | Javnu IP | Za statički dodijeljene IP adrese, možete stvoriti odgovora DNS zapis. Za dinamično dodijeljene IP adrese, morate stvoriti CNAME zapis koji se preslikava _cloudapp.net_ naziv. To pravilo primjenjuje se na VMs stvoriti na portalu klasični jer primjenjuju kao neki servis u oblaku. Dodatne informacije potražite u članku [Konfiguriranje prilagođenog naziva domene u servise u Oblaku](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Aplikacije servisa | Vanjski IP | Za vanjske IP adrese, možete stvoriti odgovora DNS zapis. U suprotnom, morate stvoriti CNAME zapis koji se preslikava azurewebsites.net naziv. Dodatne informacije potražite u odjeljku [Mapiranje prilagođenog naziva domene u aplikaciju programa Azure](../app-service-web/web-sites-custom-domain-name.md) |
| Voditelj resursa VMs | Javnu IP | Voditelj resursa VMs može imati javnu IP adrese. VM s javnu IP adresa mogu biti iza raspoređivača opterećenja. Možete stvoriti DNS-A ili CNAME zapis za adresu javnog. Ovaj prilagođeni naziv mogu se zaobići VIP na raspoređivača opterećenja. |
| Klasični VMs | Javnu IP | Klasični VMs stvoren pomoću komponente PowerShell ili EŽA moguće je konfigurirati s dinamičke ili statičke (rezervirane) virtualne adresom. Stvorite CNAME DNS-a ili zapisa, odnosno. |
