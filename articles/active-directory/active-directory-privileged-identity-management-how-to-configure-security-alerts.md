<properties
   pageTitle="Upute za konfiguriranje sigurnosnih upozorenja | Microsoft Azure"
   description="Saznajte kako konfigurirati sigurnosnih upozorenja za nastavak povlaštene upravljanje identitetom Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Konfiguriranje sigurnosnih upozorenja u Azure AD povlaštene upravljanje identitetom

## <a name="security-alerts"></a>Sigurnosna upozorenja
Azure povlaštene identiteta Management (PIM) stvara upozorenja kada postoji sumnjiva ili nesigurnih aktivnosti u svom okruženju. Kada se pokrene upozorenja, prikazuje na nadzornoj ploči za PIM. Odaberite upozorenje da biste vidjeli izvješća koja sadrži popis korisnika ili uloge koja ga je pokrenula upozorenje.

![Sigurnosna upozorenja nadzorne ploče PIM - snimka][1]



| Upozorenje | Pokretanje | Preporuka |
| ----- | ------- | -------------- |
| **Uloge dodjeljuju se izvan PIM** | Administrator trajno dodijeljena uloga izvan PIM sučelja. | Pregledajte Dodjela nove uloge. Budući da druge servise možete dodijeliti samo administratori trajna, promijenite u uvjete dodjelu po potrebi. |
| **Se uloge previše često aktiviraju** | Došlo je do previše reactivations isti uloge u vremenu dopušteno u postavkama. | Obratite se korisniku da biste vidjeli zašto oni su aktivirane ulogu toliko vremena. Možda je vremensko ograničenje previše skraćeno ih da biste dovršili svoje zadatke ili možda ih koristite skripte automatsku aktivaciju uloge. |
| **Uloge ne zahtijevaju višestruke provjere autentičnosti za aktivaciju** | Postoje uloge bez MFA omogućeno u postavkama. | Ne možemo zahtijevaju MFA za većinu korisnik s velikim ovlastima uloge, ali svakako potaknuli Omogućivanje MFA za aktivaciju svih uloga. |
| **Administratori ne koristite njihove povlaštene uloge** | Postoje uvjete administratori koje još niste aktivirali nedavno njihove uloge. | Pokretanje programa access pregled da biste odredili korisnika koje više nisu potrebne programa access. |
| **Previše globalni administrator** | Nema više globalni administratori ne preporučuje se. | Ako imate velik broj globalni administratori, vjerojatno je korisnicima se prikazuju dodatne dozvole od koje su im potrebne. Premještanje korisnika u manje povlaštene uloge ili provjerite neke od njih uvjete za ulogu umjesto trajno dodijeljeni. |

## <a name="configure-security-alert-settings"></a>Konfiguriranje sigurnosnih postavki za upozorenja

Možete prilagoditi neke od sigurnosnih upozorenja u PIM za rad s okruženje i sigurnosnih ciljeva. Slijedite ove korake da biste dosegne plohu postavke:

1. Prijava na [portal za Azure](https://portal.azure.com/) , a zatim odaberite pločicu **Azure AD povlaštene upravljanje identitetom** na nadzornoj ploči.
2. Odaberite **upravlja povlaštene uloge** > **Postavke** > **Postavke upozorenja**.

    ![Idite na postavke sigurnosnih upozorenja][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Uloge koji se aktiviraju previše često" upozorenja

Upozorenje pokreće ako korisnik aktivira isti povlaštene uloga više puta unutar određenog razdoblja. Možete konfigurirati vremensko razdoblje i broj aktivacije.

- **Vremenskog razdoblja aktivaciju obnove**: odredite u dana, sate, minute i sekunde vremensko razdoblje koje želite koristiti za praćenje sumnjiva obnove.

- **Broj aktivacija obnove**: Navedite broj aktivacije od 2 do 100 koja smatrate worthy s upozorenjem o unutar vremenskog razdoblja koje ste odabrali. To možete promijeniti postavku pomicanje klizača ili upisivanjem broja u tekstnom okviru.


### <a name="there-are-too-many-global-administrators-alert"></a>"Postoje previše globalni administratori" upozorenja

PIM pokreće upozorenje ako su ispunjeni dva različitih kriterija, a možete konfigurirati oba. Najprije želite doći do određeni prag globalnog administratora. Drugo, na vaše dodjele ukupni uloga postotak mora biti globalni administratori. Ako samo zadovoljavate jednu od ovih mjera ne pojavljuje upozorenje.  

- **Najmanji broj globalni administratori**: Navedite broj globalni administratori, od 2 do 100, razmislite o nesigurnih iznos.

- **Postotak globalni administratori**: Navedite postotak administratorima koji su globalni administratori, 0% na 100%, koji je nesigurnih u svom okruženju.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"Administratori ne koristite njihove uloge povlaštene" upozorenja

Upozorenje pokreće ako korisnik dolazi tijekom određenog vremenskog razdoblja bez aktiviranja uloge.

- **Broj dana**: Navedite broj dana od 0 do 100 koji korisnik možete raditi bez aktiviranja uloge.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
