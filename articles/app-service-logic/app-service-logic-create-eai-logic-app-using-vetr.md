<properties
   pageTitle="Stvaranje EAI logike aplikacije pomoću VETR u aplikacijama za logike u aplikacije servisa za Azure | Microsoft Azure"
   description="Provjera valjanosti, kodiranje i pretvaranje značajke BizTalk XML services"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Stvaranje EAI logike aplikacije pomoću VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Većini scenarija integracije Enterprise aplikacije (EAI) mediate podataka između izvor i odredište. Takve scenariji često imaju zajednički skup preduvjete:

- Provjerite je li se ispravno oblikovane podatke iz različitih sustava.
- Izvođenje "traženja" ulazne podatke za donošenje odluka.
- Pretvorba podataka iz jednog oblika u drugi. Ako, na primjer, pretvorba podataka iz oblik podataka u sustavu CRM oblik podataka u sustavu ERP.
- Usmjeravanje podataka u web-mjesto željenu aplikaciju ili u okvir za sustav.

U ovom se članku prikazuje uobičajenih Integracija uzorak: "mediation jednosmjerna poruka" ili VETR (Provjera Enrich, pretvorba, usmjeravanje). Uzorak VETR mediates podataka između izvorišni entitet i odredišni entitet. Obično izvorišne i odredišne su izvora podataka.

Razmislite o web-mjesto koje prihvaća narudžbe. Korisnici objavljuju narudžbe sustav putem HTTP. U pozadini sustav provjerava ulazne podatke za ispravnost, normalizes ga i nastavi u redu čekanja za servis Bus radi daljnje obrade. Sustav vodi narudžbe isključivanje reda očekuje u određenom obliku. Stoga je tijek završetka do kraja:

**HTTP** → **provjeru** → **transformacija** → **Bus servisa**

![Osnovni VETR tijek][1]

Pomoć za sljedeće aplikacije za API BizTalk sastavljanje ovaj uzorak:

* **Pokretanje HTTP** - izvor događaja poruke okidača
* **Provjera valjanosti** - Provjeri valjanost ispravnosti ulazne podatke
* **Pretvaranje** – pretvaranja podatke iz ulaznog oblik i oblik potrebnih do sustava
* **Poveznik servisa Bus** – odredišni entitet gdje će se podaci poslati


## <a name="constructing-the-basic-vetr-pattern"></a>Izgradnje osnovni VETR uzorak
### <a name="the-basics"></a>Osnove

Na portalu Azure odaberite **+ Novo**, odaberite **Web + Mobile**, a zatim **Logike aplikacije**. Odaberite naziv, mjesto, pretplatu, grupa resursa i mjesto na kojem se radi. Grupa resursa poslužiti kao spremnici za aplikacije; Svi resursi za aplikaciju idite na istoj grupi resursa.

Nakon toga dodat ćemo okidača i akcija.


## <a name="add-http-trigger"></a>Dodavanje HTTP okidača
1. **Predlošci aplikacija logike**, odaberite **Stvaranje ispočetka**.
1. Odaberite **Ga Slušatelj HTTP** iz galerije da biste stvorili novi ga slušatelj. Nazovite ga **HTTP1**.
2. Postavljanje na **automatski slati odgovor?** postavite na false. Konfiguriranje akciju okidača postavljanjem _HTTP metoda_ _objavu_ i postavljanjem _Relativni URL_ za _/OneWayPipeline_:  
    ![Pokretanje HTTP][2]
3. Odaberite Zelena kvačica da biste dovršili okidača.

## <a name="add-validate-action"></a>Dodavanje akcija za provjeru valjanosti

Sada ćemo unesite postupaka koji se izvodi kad god se aktivira okidač – to jest, kad god se poziv primili na krajnjoj točki HTTP-a.

1. Dodavanje **Alat za provjeru XML valjanosti BizTalk** iz galerije i nazovite ih _(Validate1)_ da biste stvorili instance.
2. Konfiguriranje XSD shema za provjeru valjanosti dolazne poruke XML. Odaberite akciju _Provjeri valjanost_ i _triggers('httplistener').outputs. Sadržaja_ kao vrijednost za parametar _inputXml_ .

Sada je provjera valjanosti akcija prva akcija nakon što ga slušatelj HTTP: 

![BizTalk XML alat za provjeru valjanosti][3]

Isto tako, dodat ćemo ostatak od akcija. 

## <a name="add-transform-action"></a>Dodavanje pretvorbe akcije
Pogledajmo Konfiguriranje pretvorbe koju treba normalizirati ulazne podatke.

1. Dodavanje **Servisa transformacija BizTalk** iz galerije.
2. Da biste konfigurirali pretvaranjem Pretvorba dolazne poruke XML, odaberite akciju **transformacija** kao akciju koja će se izvršiti kada se zove se taj API. Odaberite ```triggers(‘httplistener’).outputs.Content``` kao vrijednost za _inputXml_. *Mapiranje* je neobavezan parametar jer podataka se uparuje s sve konfigurirani transformacije, a samo one koje odgovaraju shemi primjenjuju.
3. Na kraju transformacija će se pokrenuti samo ako je provjera uspješna. Da biste konfigurirali ovaj uvjet, odaberite ikonu zupčanika u gornjem desnom kutu, a zatim odaberite _Dodaj uvjet da se zadovolje_. Postavite uvjet ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk pretvaranja][4]


## <a name="add-service-bus-connector"></a>Dodavanje servisa Bus poveznika
Nakon toga dodat ćemo odredište – reda Bus servisa – pisati podatke.

1. Dodavanje **Servisa Bus Connector** iz galerije. **Naziv** skupa za _Servicebus1_, postavite **Niz za povezivanje** niz za povezivanje vašoj instanci bus servisa, **Entitet naziv** skupa _red_i preskočite **naziv pretplate**.
2. Odaberite akciju **Pošalji poruku** , a polje postavljeno na **sadržaja** za akciju _actions('transformservice').outputs. OutputXml_.
3. Polje postavljeno na **Vrstu sadržaja** *xml-a/aplikacije*:  

![Servis Bus][5]


## <a name="send-http-response"></a>Slanje odgovora HTTP
Kada završi obrada kanal, šalje natrag odgovor HTTP za uspjeh i pogreške sa sljedećim koracima:

1. Dodajte **Ga Slušatelj HTTP** iz galerije i odaberite akciju **Slanje HTTP odgovor** .
2. Postavite **ID odgovor** da biste poslali *poruku*.
2. **Odgovor sadržaja** skupa na *obradu kanal dovršeno*.
3. **Šifra stanja odgovor** *200* za označavanje u HTTP 200 redu.
4. Odaberite padajući izbornik u gornjem desnom kutu, a zatim **Dodaj uvjet da se zadovolje**.  Uvjet postavite na sljedeći izraz:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Ponovite te korake da biste poslali HTTP odgovor kao i pogreške. Promjena **uvjeta** na sljedeći izraz:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Odaberite **u redu** , a zatim **Stvaranje**.



## <a name="completion"></a>Dovršetak
Svaki put kada netko pošalje poruku u krajnju točku HTTP, pokreće aplikaciju i izvršava akcije koju ste upravo stvorili. Da biste upravljali takve aplikacijama logike stvaranje, na portalu za Azure odaberite **Pregledaj** i odaberite **Logike aplikacije**. Odaberite aplikaciju da biste vidjeli dodatne informacije.

Nekoliko korisnih članaka:

[Upravljanje i nadzor aplikacija za API-JA i poveznika](app-service-logic-monitor-your-connectors.md)  <br/>
[Praćenje aplikacija u logike](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
