<properties
    pageTitle="Azure AD Connect sinkronizacije: objašnjenje korisnika i kontakte | Microsoft Azure"
    description="U članku se objašnjava korisnika i kontakata u Azure AD Connect sinkronizaciju."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect sinkronizacije: objašnjenje korisnika i kontakata

Postoji nekoliko razloga zašto biste imali više šuma servisa Active Directory te postoji nekoliko različitih implementacije topologija. Uobičajeni modeli uključuju implementacije sustava račun resursa i GAL sync'ed šuma nakon spajanja tvrtki i nabavu. Ali čak i ako postoje čisto modela, hibridnog modeli su uobičajeni kao i. Zadana konfiguracija sinkronizirano Azure AD Connect pretpostavlja da neki određeni model, no ovisno o tome kako korisnika koji se podudaraju je odabrana u vodiču za instalaciju, možete se poštovati različite postavke.

U ovoj se temi smo će proći kroz ponašanje zadanu konfiguraciju u određenim topologija. Ne možemo će proći kroz konfiguraciju i uređivaču pravila sinkronizacije može se koristiti za pregled konfiguracije.

Postoje nekoliko općenitih pravila pretpostavlja konfiguraciju:

- Bez obzira na to redoslijed smo uvoz iz izvora aktivnog direktorija, rezultat uvijek mora biti isti.
- Aktivni račun će uvijek slali prijavu podatke, uključujući **userPrincipalName** i **sourceAnchor**.
- Onemogućeni račun će slali userPrincipalName i sourceAnchor, osim ako je povezane poštanski sandučić, ako postoji bez aktivni račun može pronaći.
- Račun s poštanskim sandučićem web povezane koristit će se nikad ne userPrincipalName i sourceAnchor. Pretpostavlja se da će aktivni račun kasnije može pronaći.
- Kontakt objekt može biti dodijeljena da Azure AD kao kontakt ili korisnik. Zaista ne znate dok sve šuma servisa Active Directory izvor obrađen.

## <a name="contacts"></a>Kontakti

Pojavljuju se kontakti koji predstavlja korisnika u različitim skupa stabala uobičajeno je nakon spajanja tvrtki i nabavu gdje rješenje GALSync je premošćivanja dva ili više šuma sustava Exchange. Kontakt objekt uvijek uključivanja iz prostora poveznik za metaverse pomoću atribut pošta. Ako je već postoji u kontakt ili korisnik objekt s istom adresom pošte, objekte spojeni zajedno. To je konfiguriran u pravilu **u iz AD – kontaktu uključivanje**. Postoji i pravilo pod nazivom **u iz AD – zajednički kontakt** programa tokove atribut da biste na metaverse atributa **sourceObjectType** s nepromjenjivim **kontakt**. To pravilo ima prednost nisku pa ako bilo koji objekt korisnika pridruženo na isti objekt metaverse, a zatim pravilo **u iz AD – uobičajeni korisnika** će doprinos vrijednost korisnika za taj atribut. S tim se pravilom taj atribut neće imati vrijednost kontakt ako pridružen nijedan korisnik i vrijednost korisnika ako najmanje jedan korisnik pronašao.

Za dodjeljivanje objekta Azure AD izlaznog pravilo **Odjava AAD – obratite se pridružite** će stvoriti kontakt objekt ako atribut metaverse **sourceObjectType** je postavljena za **kontakt**. Ako je taj atribut postavljen **korisniku**, zatim pravilo **Odjava AAD – uključivanje korisnik** će stvoriti korisničkom objektu umjesto toga.
Moguće je da objekt se promovira kontakta korisniku kad više imenika Active izvor uvoza i sinkronizirati.

Na primjer, u GALSync topologiji smo tražit će kontakta objekte za sve u drugi skup stabala kada ćemo uvesti prvi skup stabala. To će faze novi kontakt objekata u AAD poveznik. Kada smo kasnije uvoz i sinkronizacija drugi skup stabala, ne možemo će pronađite stvarnih korisnika i pridružiti postojećih metaverse objekata. Ne možemo će izbrisati kontakt objekt u AAD i umjesto toga stvorite novi korisnik objekt.

Ako imate topologije gdje korisnici i predstavljen kao kontakata, provjerite jeste li odabrali tako da odgovara korisnike na atribut pošta u vodiču za instalaciju. Ako odaberete neku drugu mogućnost, neće imati je redoslijed zavisne konfiguracije. Kontakt objekata uvijek će uključiti atribut pošta, ali objekata korisnik će samo uključiti atribut pošte ako ta mogućnost nije odabrana u vodiču za instalaciju. Koje nije moguće zatim završe s dvije različite objekte u metaverse s isti atribut pošte ako kontakt objekt uvezeni prije korisničkom objektu. Tijekom izvoza za Azure AD će izbačena pogrešku. Takvo ponašanje je dizajn, a želite označava neispravne podatke ili da topologije je nije ispravno prepoznati tijekom instalacije.

## <a name="disabled-accounts"></a>Onemogućeni računi

Onemogućeni računi se sinkroniziraju kao i Azure AD. Onemogućeni računi su uobičajeni predstavljaju resurse u sustavu Exchange, primjerice sobe za sastanke. Izuzetak je korisnicima s povezane poštanski sandučić; spomenuti prethodno, oni će nikad ne dodijelite račun za Azure AD.

Pretpostavci je koji ako nalazi se onemogućene korisnički račun, zatim smo neće pronaći neki drugi račun za aktivni kasnije i objekt je dodijeljena da Azure AD s userPrincipalName i sourceAnchor pronaći. U slučaju da neki drugi račun za aktivni uključili će na isti objekt metaverse, zatim njegove userPrincipalName i sourceAnchor će se koristiti.

## <a name="changing-sourceanchor"></a>Promjena sourceAnchor

Kada se objekt izvezena je za Azure AD, a zatim nije dopušteno da biste promijenili u sourceAnchor više. Kada izvezeni objekt atribut metaverse **cloudSourceAnchor** postavlja se s vrijednošću **sourceAnchor** prihvatio Azure AD. Ako **sourceAnchor** se promijeni, a ne odgovaraju **cloudSourceAnchor**, pravilo **Odjava AAD – uključivanje korisnik** će vratiti pogreške **sourceAnchor atribut promijenio**. U ovom slučaju konfiguraciju ili podatke potrebno ispraviti da bi se isti sourceAnchor su prisutne u metaverse ponovno prije objekt više se mogu se sinkronizirati.

## <a name="additional-resources"></a>Dodatni resursi

* [Azure AD povezivanje sinkronizacije: Mogućnosti za prilagodbu sinkronizacije](active-directory-aadconnectsync-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
