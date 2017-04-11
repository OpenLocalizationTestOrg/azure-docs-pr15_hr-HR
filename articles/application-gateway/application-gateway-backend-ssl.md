<properties
   pageTitle="Omogućivanje SSL pravila i završetka za kraj SSL na aplikaciju pristupnika | Microsoft Azure"
   description="Ova stranica sadrži pregled pristupnika aplikacija podržava end da biste završetka SSL."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Omogućivanje SSL pravila i završetka za kraj SSL na aplikaciju pristupnika

## <a name="overview"></a>Pregled

Pristupnik aplikacija podržava SSL prekid pristupnika, nakon koje promet obično teče nešifrirani s pozadinskom poslužiteljima. Time se omogućuje web-poslužiteljima biti unburdened iz indirektni skup šifriranje/dešifriranje. No za neke korisnike šifrirane komunikacije s poslužiteljima pozadinskog nije prihvatljiva mogućnost. To može biti zbog preduvjeti za sigurnost i usklađenost ili aplikacije mogu prihvatiti samo sigurnu vezu. Pristupnik za aplikaciju za takve aplikacije sada podržava šifriranja SSL završetka na kraj.

END da biste završetka SSL omogućuje sigurno prijenos osjetljivih podataka pozadinskog šifrirane zamrznuli snimanja prednosti prednosti značajki za ujednačavanje opterećenja 7 sloj koji pristupnika aplikacije pruža, kao što su afinitet kolačića, koji se temelji na URL usmjeravanje, podršku za usmjeravanje prema web-mjesta ili mogućnost ubaciti X-proslijeđenih-* zaglavlja.

Kada je konfiguriran za korištenje način komunikacije SSL end da biste završi, pristupnik za aplikaciju prekida SSL sesija pri pristupnika i dešifrira promet korisnika. Primjenjuje se zatim konfigurirani pravila da biste odabrali odgovarajući pozadinskog instance skup za usmjeravanje prometa. Pristupnik za aplikaciju zatim pokreće SSL veze s poslužiteljem pozadinske i ponovno šifrira podataka pomoću certifikata javnog ključa pozadinskog poslužitelja prije prijenosom zahtjev za pozadinski. Završi da biste završili SSL omogućena je tako da postavite postavku protokola u BackendHTTPSetting da biste Https, koji se primjenjuje pozadinskog resurse. Svaki pozadinskog poslužitelja u pozadinskog s end da biste završetka SSL omogućen mora biti konfigurirana s potvrdom da biste omogućili sigurnu komunikaciju.

![scenarij ssl END da biste završetka][1]

U ovom se primjeru korištenjem TLS1.2 usmjeruje se u pozadinskog poslužitelji u Pool1 putem SSL-a završetka i završetka.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Završi da biste završili SSL i stvaranja popisa dopuštenih stavki certifikata

Pristupnik za aplikaciju samo komunicira s poznati pozadinskog instance koje ste whitelisted svoj certifikat s računala pristupnika. Da biste omogućili stvaranja popisa dopuštenih stavki certifikata, morate prenesite javni ključ pozadinskog poslužitelja certifikati aplikacije pristupnika (nije korijenski certifikat). Zatim dopušteno samo veze na poznati i whitelisted pozadinski sustav. Preostali pozadinski sustav rezultira pristupnika pogreške. Samopotpisane potvrde su svrhe test samo i ne preporučuje se radnih opterećenja proizvodnje. Takve potvrde moraju također biti whitelisted s pristupnika aplikacije kao što je opisano u prethodne korake prije nego što mogu se koristiti.

## <a name="application-gateway-ssl-policy"></a>Pravilnik SSL pristupnika aplikacije

Pristupnik aplikacija podržava korisnik može konfigurirati SSL Pregovori pravila, koje omogućuju veću kontrolu nad SSL veze kupci pristupnika za aplikacije.

1. SSL 2.0 i 3.0 onemogućio zadane za sve aplikacije pristupnika. Nisu konfigurirati uopće.
2. SSL pravila definicija daje mogućnost da biste onemogućili neku od sljedećih 3 protokola - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Ako je definiran nema pravila SSL sva tri (TLSv1\_0, TLSv1\_1, TLSv1_2) omogućeni.

## <a name="next-steps"></a>Daljnji koraci

Nakon prepoznavanju end da biste završetka SSL i SSL pravila, otvorite da biste [omogućili SSL end da biste završetka na pristupnika aplikacije](application-gateway-end-to-end-ssl-powershell.md) da biste stvorili pristupnik za aplikaciju s mogućnošću da biste poslali promet pozadinski sustav šifrirane obrascu.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png