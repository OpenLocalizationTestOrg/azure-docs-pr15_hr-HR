<properties 
    pageTitle="Dodavanje potvrde u trgovini Java CA | Microsoft Azure" 
    description="Saznajte kako dodati certifikat za izdavanje certifikata (CA) certifikata Java CA spremište certifikata (cacerts) za servis Twilio ili Bus servisa Azure." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Dodavanje certifikat u spremištu Java ustanove za Izdavanje certifikata
Sljedeći koraci pokazuju kako da biste dodali certifikat za izdavanje certifikata (CA) certifikat spremište certifikata (cacerts) Java CA. U primjeru koristi se ustanove za Izdavanje certifikata potrebnih Twilio servisa. Informacije navedene u nastavku temi opisuje kako instalirati ustanove za Izdavanje certifikata za Bus servisa Azure. 

Keytool možete koristiti za dodavanje ustanove za Izdavanje certifikata prije no što zipanju vaše JDK i dodavanje Azure projekta **approot** mapu ili možete pokrenuti zadatak programa Azure pokretanja koji koristi keytool da biste dodali certifikata. U ovom se primjeru pretpostavlja da dodate certifikat CA prije JDK se zipane. Osim toga, određene ustanove za Izdavanje certifikata će se koristiti u ovom primjeru, no korake dobivanje različite ustanove za Izdavanje certifikata i uvoz u trgovini cacerts bi biti sličan.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Da biste dodali certifikat u spremištu cacerts

1. U naredbeni redak koji je postavljen vaš JDK **jdk\jre\lib\security** mapu, pokrenite sljedeće da biste vidjeli koje su instalirati:

    `keytool -list -keystore cacerts`

    Će se zatražiti lozinka trgovine. Lozinka zadani je **changeit**. (Ako želite promijeniti lozinku, potražite u dokumentaciji keytool pri <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) U ovom se primjeru pretpostavlja da potvrdu s MD5 otisaka prstiju 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nije naveden na popisu, a koje želite uvesti (određeni certifikat potreban je servis Twilio API-JA).
2. Nabavite certifikat na popisu certifikati navedeni na [GeoTrust korijenskih certifikata](http://www.geotrust.com/resources/root-certificates/). Desnom tipkom miša kliknite vezu za potvrdu s 35:DE:F4:CF serijski broj i spremite ga u mapu **jdk\jre\lib\security** . Za potrebe ovog primjera, prije spremanja u datoteku pod nazivom **Equifax\_sigurno\_certifikat\_Authority.cer**.
3. Uvoz certifikata putem sljedeću naredbu:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Kada se to zatraži vjeruj ovom certifikatu ako certifikat nema MD5 otiska prsta 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, odgovor tako da upišete **y**.
4. Pokrenite sljedeću naredbu da biste bili sigurni da je uspješno uvezeni certifikat CA:

    `keytool -list -keystore cacerts`

5. Poštanski broj u JDK i dodajte ga u mapu **approot** Azure projekta.

Informacije o keytool potražite u članku <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure korijenskih certifikata

Vaše aplikacije koje koriste Azure usluge (primjerice Bus servisa Azure) moraju vjerovati Zagrebu CyberTrust korijenski certifikat. (Početak Travanj 15, 2013 Azure počeli Migracija s GTE CyberTrust globalni Root CyberTrust Root Zagrebu. U ovom migracije trajalo nekoliko mjeseci za dovršetak.)

Zagrebu certifikat možda već instaliran u spremištu cacerts, stoga Imajte na umu da biste pokrenuli u **keytool-popis** naredba prvi put da biste vidjeli ako već postoji.

Ako je potrebno dodati CyberTrust Root Zagrebu sadrži 02:00:00:b9 serijski broj i SHA1 otisak prsta d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Mogu se preuzeti sa <https://cacert.omniroot.com/bc2025.crt>, spremiti lokalnu datoteku s nastavkom **.cer**, a zatim uvoza pomoću **keytool** kao što je prikazano gore.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korijenskih certifikata koji se koriste Azure potražite u članku [Azure korijenski certifikat migracije](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Dodatne informacije o Java potražite u članku [Razvojni centar za Java](/develop/java/).
