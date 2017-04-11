<properties 
   pageTitle="Upravitelj StorSimple servisa nadzorne ploče - virtualne polja | Microsoft Azure"
   description="U članku se opisuje nadzorna ploča za StorSimple Upravitelj servisa i objašnjava kako ga koristiti za praćenje stanja sustava StorSimple virtualne polja."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Korištenje nadzorna ploča za StorSimple Upravitelj servisa za polja virtualne StorSimple

## <a name="overview"></a>Pregled

Stranice nadzorne ploče StorSimple Upravitelj servisa nudi sažet pregled StorSimple virtualne poljima (poznat i kao StorSimple lokalnog virtualne uređaja ili virtualne uređaji) koji su povezani sa servisom StorSimple Upravitelj isticanje onih koje je potrebno obratiti pozornost administratora sustava. Pomoću ovog praktičnog vodiča predstavlja stranice nadzorne ploče, objašnjava sadržaja nadzorne ploče i – opis funkcije i opisuju se zadaci koje možete izvršiti s ove stranice.

![Nadzorna ploča servisa](./media/storsimple-ova-service-dashboard/dashboard1.png)

Nadzorna ploča za StorSimple Upravitelj servisa prikazuju se sljedeći podaci:

- U područje **grafikona** , pri vrhu stranice, vidjet ćete odgovarajuće metriku za virtualne uređaje. Možete pogledati primarni prostora za pohranu koriste preko svih virtualne uređaja, kao i za pohranu u oblaku troše virtualne uređaji vremenskom razdoblju. Pomoću kontrola u gornjem desnom kutu grafikona da biste odredili način korištenja relativne i apsolutne i da biste vidjeli 1 tjedan, 1 mjesec, 3 mjeseca ili godine 1 vremenskoj skali. Korištenje kontrola osvježavanja ![kontrola osvježavanja](./media/storsimple-ova-service-dashboard/refresh-control.png) da biste osvježili grafikona.

- **Pregled korištenja** prikazuje primarni prostora za pohranu koji je dodijeljen i troše sve virtualne uređaji odnosu ukupni spremišta dostupnog putem svih virtualne uređaja. **Provisioned** se odnosi na količinu prostora za pohranu koji je pripremili i dodijeliti za korištenje, **korišteni** se odnosi na korištenje zajedničke stavke i količine kao što je prikazati Pokretači koji su povezani s virtualne uređaje i **Najveći kapaciteta** prikazuje najveći kapacitet svih virtualne uređaja.

- Područje **upozorenja** omogućuje snimke aktivnog upozorenja putem svih virtualne uređaja grupirano po upozorenja težinu. Klikom na razinu težinu otvara se stranici **upozorenja** iz djelokruga da biste prikazali one upozorenja. Na stranici **upozorenja** možete kliknuti pojedinačne upozorenja da biste vidjeli dodatne detalje o tom upozorenja, uključujući preporučene akcije. Upozorenja možete isključiti i ako je problem riješen.

- Područje **Poslovi** omogućuje snimku nedavne poslova preko svih virtualne uređaja povezanih s na servisu. Postoje veze koje možete koristiti za pregled zadataka koji su trenutno u tijeku i one koje uspjeli u zadnja 24 sata. 

- Područje **brzi pogled** na desnoj strani stranice sadrži korisne informacije kao što su ukupni broj virtualne uređaja povezanih s servis, mjesto servisa, a zatim Detalji pretplate koja je povezana sa servisom stanje servisa. Postoji vezu da biste se prijavili operacije. Kliknite vezu da biste vidjeli popis svih dovršene operacija StorSimple Upravitelj servisa. 

Stranice nadzorne ploče StorSimple Upravitelj servisa možete koristiti da biste započeli sljedeće zadatke:

- Pronađite ključa za registraciju servisa.
- U zapisnicima operacija.

## <a name="get-the-service-registration-key"></a>Registracija ključ servisa

Servis ključa za registraciju se koristi da biste registrirali StorSimple virtualnog uređaja sa servisom StorSimple Upravitelj tako da se uređaj pojavi na portalu Azure klasični za daljnje upravljanje akcije. Ključ se stvara na prvom uređaju virtualne i zajednički koristiti s preostale virtualne uređaje. 

Klikom **Ključa za registraciju** (pri dnu stranice) otvara se dijaloški okvir **Ključa za registraciju** koju možete trenutni ključ usluga registracije kopiranje u međuspremnik ili Obnovi ključa za registraciju servisa.

![ključa za registraciju](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Ponovno stvaranje tipku ne utječe na prethodno registrirani virtualne uređaji: utječe samo virtualne uređaje registrirane sa servisom kada je regenerated tipku.

Dodatne informacije o tome kako izvući ključa za registraciju servisa, idite na [Dohvati ključa za registraciju servisa](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>U zapisnicima operacije

Zapisnicima operacija tako da kliknete vezu zapisnika za operaciju u oknu za **brzi pogled** na nadzornoj ploči. Ta će vas stranicu za upravljanje servisima, gdje možete filtrirati i pogledajte zapisnike određene usluzi StorSimple Upravitelj.

![Operacije zapisnika](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako [koristiti lokalne web korisničkog Sučelja za administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).