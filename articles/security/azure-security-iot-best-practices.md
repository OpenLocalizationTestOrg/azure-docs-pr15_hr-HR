<properties
   pageTitle="Internet stvari sigurnost najbolje prakse | Microsoft Azure"
   description="Članak sadrži curated popis Microsoft Internet stvari sigurnost najbolje prakse i općenite preporuke."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Internet stvari sigurnost najbolje prakse

Zaštita infrastrukture Internet stvari (IoT) je ključnih undertaking za sve vezanih uz IoT rješenja. Zbog broj uređaja koji je uključen i raspodijeljeno prirode te uređaje, utjecaja sigurnosnog događaja vezanih uz ugroziti od milijune IoT uređaja je koji nisu trivial i mogu imati utjecaj Primamo.

Zbog toga IoT sigurnost mora sigurnost u dubine pristup. Podataka mora biti siguran u oblak i kao prebacuje privatne i javne mreže. Načini moraju biti na mjestu sigurno Dodjela uređaji IoT sami. Svaki sloja, na uređaju, s mrežom na cloud pozadinske mora jamstva istaknuti sigurnost.

Najbolje prakse IoT može se podijeliti na sljedeći način:

- Proizvođač hardvera IoT ili integrator
- IoT rješenja za razvojne inženjere
- Deployer IoT rješenja
- Operator IoT rješenja

U ovom članku navedene su [Internet od stvari sigurnost najbolje prakse](../iot-suite/iot-security-best-practices.md). Pogledajte članak detaljnije informacije.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Proizvođač hardvera IoT ili integrator

Ako ste je proizvodnja IoT hardver ili hardver integrator, slijedite dolje najbolje prakse:

- **Opseg hardver minimalni preduvjeti**: dizajna hardver uključuje minimalne značajke potreban za operaciju hardver i ništa više. 
- **Provjerite hardver mijenjati dokaz**: Izrada u mehanizme za otkrivanje neovlašteno mijenjanje fizičke hardvera, kao što su otvaranje naslovnica uređaj, uklonite dio uređaja, itd. 
- **Sastavljanje oko sigurne hardver**: Ako vam dopuštaju [troška prodane robe](https://en.wikipedia.org/wiki/Cost_of_goods_sold) , sastavljanje sigurnosnim značajkama kao što su sigurne i šifrirane prostora za pohranu i sustavom pouzdana Platform Module TPM pokretanje funkcije.
- **Provjerite nadogradnje sigurne**: Nadogradnja firmver tijekom života uređaja je inevitable.

## <a name="iot-solution-developer"></a>IoT rješenja za razvojne inženjere

Ako ste je razvojni inženjer rješenja IoT, slijedite dolje najbolje prakse:

- **Praćenje sigurne softver razvoj methodology**: razvoj sigurne softver potreban dna naviše misle o sigurnosti od početka projekta s njegova implementacija, testiranje i implementaciju.
- **Odaberite Otvori izvor softver s Oprez**: Otvori izvor softvera nudi mogućnost razvijati rješenja.
- **Integrate s Oprez**: mnoge pogreške sigurnost softver na rub biblioteke i API-postoji. 

## <a name="iot-solution-deployer"></a>Deployer IoT rješenja

Ako ste u deployer IoT rješenja, slijedite najbolje prakse ispod:

- **Hardverski implementacije sigurno**: IoT implementacijama možda ćete morati hardver da biste se uvesti u nesigurnom mjesta kao javno razmake ili unsupervised regionalne sheme.
- **Zadrži provjere autentičnosti tipke sigurnih**: tijekom uvođenja, svakom uređaju zahtijeva uređaju ID-a i povezane provjere autentičnosti tipke generira servisa u oblaku. Zadrži ove tipke fizički sigurnom čak i nakon uvođenje. Bilo koju compromised tipku mogu se zlonamjerni uređaj maskirati kao postojeću datoteku.

## <a name="iot-solution-operator"></a>Operator IoT rješenja

Ako ste operator rješenje IoT, slijedite najbolje prakse ispod:

- **Sustavi Ostanite u tijeku**: Provjerite je li uređaj operacijski sustavi i sve upravljačke programe će se ažurirati na najnovije verzije. 
- **Zaštita od zlonamjernog aktivnosti**: Ako operacijski sustav dopušta, postavite najnovije mogućnosti antivirusni i zlonamjernog na svaki uređaj operacijski sustav. 
- **Često nadzora**: nadzor IoT infrastrukture za sigurnost povezane probleme prilikom odgovaranja sa sigurnošću je ključ.
- **Fizički zaštita infrastrukture IoT**: najgorim sigurnosnim napadima protiv infrastrukture IoT su pokrenuti pomoću fizičke pristup uređaja.
- **Zaštita oblaka vjerodajnice**: oblaka vjerodajnice za konfiguriranje i radi implementacije sustava IoT su najlakše ćete dobiti pristup i ugroziti u sustavu IoT. 
