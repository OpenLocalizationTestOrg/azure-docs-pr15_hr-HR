<properties
   pageTitle="Izrada prebacivanje oporavak i uređaja za vaše StorSimple virtualne polja"
   description="Saznajte više o upute za prebacivanje na StorSimple virtualne polja."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Izrada prebacivanje oporavak i uređaja za vaše StorSimple virtualne polja


## <a name="overview"></a>Pregled

U ovom se članku opisuje oporavak Izrada za sustava Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaj) uključujući detaljne korake potrebne za uspjeti putem nekog drugog uređaja virtualne slučaju na Izrada. Na prebacivanje omogućuje vam za migraciju podataka iz *izvora* uređaj s podatkovnim centrom na drugi uređaj *cilj* koji se nalaze u istim ili različitim geografski. Prebacivanje uređaj je za cijelo uređaj. Tijekom prebacivanje, podataka oblaka za uređaj izvor mijenja vlasništvo koji ciljni uređaj.

Prebacivanje uređaj je orchestrated putem značajke Izrada oporavak (DR) i pokreće na stranici **uređaja** . Ova stranica tabulates svih StorSimple uređaja povezanih s StorSimple Upravitelj usluge. Za svaki uređaj neslužbeni naziv, stanje, Dodjela resursa i maksimalne kapacitetu, vrsta i model prikazuju.

![](./media/storsimple-ova-failover-dr/image15.png)

U ovom se članku samo je dodijeljen StorSimple virtualne polja. Neuspjeh na uređaju sa sustavom 8000 niz, idite na [Prebacivanje i oporavak Izrada StorSimple uređaja](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Što je Izrada oporavak?

U scenariju za oporavak (DR) s Izrada primarni uređaj prestane radi. U tom slučaju možete premjestiti u oblak podataka vezanih uz nije uspjelo uređaja na drugi uređaj pomoću primarnog uređaja kao *izvor* i navođenjem neki drugi uređaj kao *cilj*. Ovaj postupak se nazivaju se *Prebacivanje*. Tijekom prebacivanje, na jedinicama ili dionice s uređaja izvora promijenite vlasništvo i prenose se na ciljni uređaj. Bez filtriranja podataka je dopušteno.

DR je katalog modeliran kao vraćanja cijelog uređaja pomoću sustava ekstrema koji se temelji na kartu – tiering i praćenje. Karte ekstrema definiran dodjeljivanjem ekstrema vrijednost podataka koji se temelje na čitanje i pisanje uzoraka. Ovaj ekstrema mapirati zatim razine najniže blokova podataka ekstrema u oblak najprije zadržavanje podataka blokova visoke ekstrema (najčešće koriste) lokalni sloju. Tijekom DR, karte ekstrema koristi se za vratiti, a zatim rehydrate podatke iz oblaka. Uređaj dohvaćanja sve na količine i zajedničko korištenje u zadnje sigurnosne kopije nedavne (kao što je određeno interno) i izvodi vraćanje iz sigurnosne. Cijeli postupak DR orchestrated je uređaj.


## <a name="prerequisites-for-device-failover"></a>Preduvjeti za prebacivanje uređaja


### <a name="prerequisites"></a>Preduvjeti

Za prebacivanje bilo kojeg uređaja mora zadovoljavati sljedeće preduvjete:

- Izvor uređaj mora biti u stanju **Deactivated** .

- Ciljni uređaj mora prikazivati kao **aktivna** na portalu za Azure klasični. Morat ćete Dodjela cilj virtualnog uređaja kapaciteta isti ili noviji. Zatim koristite lokalni web korisničko Sučelje za konfiguriranje i uspješno registrirati virtualnog uređaja.

    > [AZURE.IMPORTANT] Pokušajte da biste konfigurirali registrirani virtualnog uređaja pomoću usluge tako da kliknete **Dovršeno uređaja**. Nema Konfiguracija uređaja treba izvršiti pomoću usluge.

- Uređaj izvorišno i odredišno moraju biti iste vrste. Samo uspijeva putem virtualne uređaj konfiguriran kao poslužitelj datoteka na drugi poslužitelj datoteka. Isto vrijedi i za poslužitelj iSCSI.

- Za poslužitelj datoteka DR, preporučujemo da se pridružite ciljni uređaj na istom domenu kao izvorišnog web-mjesta tako da se automatski riješi dozvole za zajedničko korištenje. U ovom izdanju podržana je samo prebacivanje ciljni uređaj u istoj domeni.

### <a name="other-considerations"></a>Druge napomene

- Preporučujemo da se snimljene količine i zajedničko korištenje na uređaju izvor izvanmrežno.

- Ako je planiranog prebacivanje, preporučujemo da iskoristite sigurnosnu kopiju uređaja, a zatim prijeđite na prebacivanje da biste minimizirali gubitka podataka. Ako je neplanirano prebacivanje, najnovija sigurnosna kopija će se koristiti za vraćanje uređaja.

- Dostupne cilj uređaji za DR su uređaja koji imaju isti ili veća kapaciteta u usporedbi s uređaja izvora. Neće biti dostupni kao ciljnih uređaje koji su povezani s usluge, ali ne ispunjavaju kriterije dovoljno prostora.

### <a name="dr-prechecks"></a>DR prechecks

Prije početka na DR, prechecks se izvode na uređaju. Te su provjere osigurati bez pogrešaka dolazi kada DR commences. Na prechecks obuhvaćaju sljedeće:

- Provjera valjanosti računa za pohranu

- Provjera povezivost oblaka za Azure

- Provjera dostupnog prostora na uređaju cilj

- Provjera ako uređaju izvorni poslužitelj sa sustavom iSCSI ima valjani nazivi ACR IQN (ne prelazi 220 znakova) i lozinku CHAP (12 i 16 znakova) pridružene jedinica

Ako bilo koji od gore prechecks uspjeti, ne možete nastaviti s na DR. Morate riješiti te probleme i ponovno pokušajte DR.

Po dovršetku na DR uspješno vlasništvo nad oblaka podataka na uređaju izvor prenose se u ciljni uređaj. Uređaj izvor pa više nije dostupan na portalu. Pristup sve na količine i zajedničko korištenje na uređaju izvor blokiran, a ciljni uređaj postaje aktivna.

> [AZURE.IMPORTANT]
> 
> Iako uređaj više nije dostupan, virtualnog računala koja vam dodijeljena u sustavu glavno računalo i dalje koristi resurse. Kad u DR je uspješno dovršena, možete izbrisati ovaj virtualni stroj iz sustava glavnog računala.

## <a name="fail-over-to-a-virtual-array"></a>Nije uspjelo putem virtualne polja

Preporučujemo da imate li drugi StorSimple virtualne polja dodjeli konfiguriran putem lokalnog web korisničkog Sučelja i registrirana sa servisom StorSimple upravitelj prije pokretanja ovog postupka.


> [AZURE.IMPORTANT]
> 
> - Nije dopušteno se neće putem s uređaja niz StorSimple 8000 1200 virtualnog uređaja.
> - Možete se neće putem iz Savezna informacije Processing Standard (FIPS) omogućena virtualnog uređaja uvesti u portal državne virtualne uređaj Azure klasični portalu. Na vrijedi i obrnuto.

Izvršite sljedeće korake da biste vratili uređaj cilj virtualnog uređaja za StorSimple.

1. Iskoristite količine i zajedničko korištenje izvan mreže na glavnom računalu. Pogledajte upute za operacijski sustav – određene na glavnom računalu za izvanmrežni jedinicama/zajedničko korištenje. Ako nije već izvanmrežni način rada, morat ćete poduzeti sve na količine/dionice izvanmrežno na uređaju tako da odaberete **uređaje > zajedničko korištenje** (ili **uređaj > količine**). Odaberite zajedničko korištenje/glasnoću i kliknite **Preuzimanje izvanmrežnog** na dno stranice. Kada se zatraži potvrda, kliknite **da**. Ponovite taj postupak za sve dionice/jedinice na uređaj.

2. Na stranici **uređaja** odaberite izvor uređaj za prebacivanje, a zatim kliknite **Deaktiviraj**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Zatražit će se za potvrdu. Deaktivacija uređaj je trajno postupak koji nije moguće poništiti. Će i podsjetiti da bi vaša mrežna/količine izvanmrežno na glavnom računalu.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Nakon potvrde, počet će se Deaktivacija. Nakon što u deaktivacija uspješno završi, bit ćete obaviješteni.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Na stranici **uređaja** stanje uređaja sada promijenit će **Deactivated**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Odaberite deaktiviran uređaj, a zatim pri dnu stranice kliknite **Prebacivanje**.

6. U čarobnjaku prebacivanje potvrda koji se otvara, učinite sljedeće:

    1. S padajućeg popisa dostupnih uređaja, odaberite je **ciljni uređaj.** Samo uređaje koje ste dovoljno kapaciteta prikazuju se na padajućem popisu.

    2. Pregledajte pojedinosti pridruženi uređaju izvora kao što su naziv uređaja, ukupni kapacitet i nazivi dionice koji će nije uspjela tijekom.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Provjerite je **li slažete prebacivanje je trajno operacija, a kada se prebacivanje uspješno završi, izbrisat će se uređaj izvora**.

8. Kliknite ikonu provjeri ![](./media/storsimple-ova-failover-dr/image1.png).


9. Prebacivanje posao će se inicirati i bit ćete obaviješteni. Kliknite **Prikaz posao** praćenje u prebacivanje.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. Na stranici **Poslovi** vidjet ćete posao prebacivanje stvorene za uređaj izvora. Ovaj zadatak obavlja DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Nakon uspješne DR prechecks posao prebacivanje će rezultiraju vraćanja zadatke za svaki zajedničko korištenje/glasnoću koji postoji na uređaju izvora.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Po dovršetku za prebacivanje na stranici **uređaja** .

    na. Odaberite StorSimple virtualnog uređaja koji je korišten kao ciljni uređaj za prebacivanje proces.

    b. Otvorite stranicu **dionice** (ili **količine** ako iSCSSI poslužiteljem). Sve na zajedničko korištenje (jedinice) s uređaja stare sada mora biti naveden.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Dostupno za videozapis**

U ovom se videozapisu pokazuje kako uspjeti putem lokalnog StorSimple virtualnog uređaja na drugi uređaj virtualne.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Oporavak Izrada tvrtke continuity (BCDR)

Scenarij oporavak (BCDR) Izrada continuity tvrtke pojavljuje kada cijelu Azure podatkovnog centra zaustavlja radi. To može utjecati na servis za Upravitelj StorSimple i pridruženi StorSimple uređaji.

Ako nema StorSimple uređaja koji su registrirani prije došlo je do na Izrada, ti uređaji StorSimple možda ćete morati izbrisati. Nakon Izrada, možete stvoriti i konfigurirati te uređaje.

## <a name="errors-during-dr"></a>Pogreške tijekom DR

**Oblak povezivanje nedostupnosti tijekom DR**

Ako povezivanje oblaka nadziranje prekinuto kada je pokrenut DR prije dovršetka vraćanja uređaj, na DR neće uspjeti i bit ćete obaviješteni. Odredišni uređaj koji je korišten za DR pa je označena kao *nestabilan.* Iste ciljni uređaj ne mogu se zatim koristiti za buduće DRs.

**Nema kompatibilan cilj uređaja**

Ako uređaja dostupni cilj nije dovoljno prostora, prikazat će pogrešku efekt da nema uređaja kompatibilne cilj.

**Precheck pogreške**

Ako nešto na prechecks nisu zadovoljeni, će biti navedeno precheck pogreške.

## <a name="next-steps"></a>Daljnji koraci

Saznajte više o tome kako [upravljati vaše StorSimple polja virtualne putem lokalnog weba korisničkog Sučelja](storsimple-ova-web-ui-admin.md).
